ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM arm32v7/buildpack-deps:bookworm-curl AS installer

# Retrieve .NET Runtime
RUN dotnet_version=8.0.17 \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-arm.tar.gz \
    && dotnet_sha512='bb61bb5e5deb7fbc030c087fda603bdfc12a8333fb40aebbe760dfd5c4a49a4a12b2a603aa1ce16f666064d6f93aec12fe16f7136e34b4f5e1797e1873984f5c' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:8.0.17-bookworm-slim-arm32v7

# .NET Runtime version
ENV DOTNET_VERSION=8.0.17

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
