FROM findtune/builder-base

# Copy redis into container
COPY . /app/

# Specify build/configure and plugin activation point
RUN {                                                                           \
  echo '#!/bin/bash';                                                           \
  echo 'cd /app';                                                               \
  echo 'export META_IGNORE_LIST="${META_IGNORE_LIST};conftest.c;conftest.cpp"'; \
  echo 'export META_PLUGIN_ACTIVATED="Y"';                                      \
  echo 'make -j$(nproc)';                                                       \
} > /mlos-build.sh \
  && chmod +x /mlos-build.sh

# Entrypoint runs build w/ plugin active
ENTRYPOINT [ "/mlos-build.sh" ]
