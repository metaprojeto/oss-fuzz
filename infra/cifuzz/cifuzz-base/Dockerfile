# Copyright 2020 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-runner

RUN apt-get update && \
    apt-get install systemd -y && \
    wget https://download.docker.com/linux/ubuntu/dists/focal/pool/stable/amd64/docker-ce-cli_20.10.8~3-0~ubuntu-focal_amd64.deb -O /tmp/docker-ce.deb && \
    dpkg -i /tmp/docker-ce.deb && rm /tmp/docker-ce.deb

# Install newer Python from base-builder.
COPY --from=gcr.io/oss-fuzz-base/base-builder /usr/local/bin/python3 /usr/local/bin/python3
COPY --from=gcr.io/oss-fuzz-base/base-builder /usr/local/lib/libpython3* /usr/local/lib/
COPY --from=gcr.io/oss-fuzz-base/base-builder /usr/local/lib/python3.8 /usr/local/lib/python3.8
RUN ldconfig

ENV OSS_FUZZ_ROOT=/opt/oss-fuzz
ADD . ${OSS_FUZZ_ROOT}
RUN python3 -m pip install -r ${OSS_FUZZ_ROOT}/infra/cifuzz/requirements.txt

# Python file to execute when the docker container starts up.
# We can't use the env var $OSS_FUZZ_ROOT here. Since it's a constant env var,
# just expand to '/opt/oss-fuzz'.
ENTRYPOINT ["python3", "/opt/oss-fuzz/infra/cifuzz/cifuzz_combined_entrypoint.py"]
