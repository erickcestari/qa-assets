# Runs the script on the minimal Ubuntu it expects:
#
#   docker build -t delete-nonreduced-fuzz-inputs delete-nonreduced-fuzz-inputs
#   docker run --rm -v "$PWD/run:/work" delete-nonreduced-fuzz-inputs [--extra-ref=<ref>]...
#
# The pruned qa-assets checkout, with one commit per step, ends up in run/qa-assets. Use a new
# run dir for every run.

FROM ubuntu:26.04 AS build
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y cargo
COPY Cargo.toml Cargo.lock /src/
COPY src /src/src
RUN cargo install --locked --path /src --root /usr/local

# Only the binary is copied over, so the run happens on a clean image like a fresh VM.
FROM ubuntu:26.04

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y ca-certificates git \
    && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/bin/delete-nonreduced-fuzz-inputs /usr/local/bin/
WORKDIR /work
ENTRYPOINT ["delete-nonreduced-fuzz-inputs"]
