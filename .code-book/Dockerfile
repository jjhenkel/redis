# Maybe we have already built once, and if that's the 
# case we probably have a compilation database we can reuse
# so let's get a reference to our previous image
ARG CB_IMAGE_NAME=code-book/cli
FROM ${CB_IMAGE_NAME} as previous

# Use the base image we provide
FROM code-book/cli as final

# Copies source code into container
COPY . /app/

# Maybe copy existing compile_commands.json
COPY --from=previous /ignore.txt /build/compile_commands.json* /build/

# This can optionally be set to force re-capturing
# the compilation database
ARG CB_FORCE_CAPTURE

# Generate a compilation database (compile_commands.json) 
# if needed (checks via code-book needs-capture ...)
RUN \
  # Either we dont need capture
  code-book needs-capture \
    ${CB_FORCE_CAPTURE:+'--force'} \
    /build/compile_commands.json \
  || (\
    # Or we do, so configure and 'capture' make
    cd /app \
    && code-book capture -- \
      make -j$(nproc) \
    && mv compile_commands.json /build/compile_commands.json \
  )

# Redis needs this to compile later
RUN cd /app/src && make persist-settings

# Entrypoint runs code-book ingest
ENTRYPOINT [ "code-book", "ingest", "/build/compile_commands.json" ]
