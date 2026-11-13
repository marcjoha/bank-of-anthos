# Copyright 2022 Google LLC
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#      http://www.apache.org/licenses/LICENSE-2.0
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM cypress/included:13.17.0@sha256:128490f34f1f2f7c0bab4dd1dbc4eaf0435ba99fe3e0f538d8ecb98b92e34d0f

WORKDIR /e2e
COPY . .

# install curl and gpg which are needed for fetching the Google Cloud GPG key
RUN apt-get update && apt-get install -y --no-install-recommends curl gpg ca-certificates

# install gcloud cli tools to get kubectl context and service/ingress endpoint ip to test
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee /etc/apt/sources.list.d/google-cloud-sdk.list \
    && curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg \
    && apt-get update -y \
    && apt-get install -y --no-install-recommends google-cloud-cli google-cloud-sdk-gke-gcloud-auth-plugin kubectl \
    && rm -rf /var/lib/apt/lists/*

ENV USE_GKE_GCLOUD_AUTH_PLUGIN=True
ENV XDG_CONFIG_HOME=/e2e

# run custom bash script to set CYPRESS_baseUrl and execute tests
ENTRYPOINT [ "/bin/bash", "-c", "./run_for_env.sh" ]
