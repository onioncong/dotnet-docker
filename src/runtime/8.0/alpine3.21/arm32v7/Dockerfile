ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:8.0.17-alpine3.21-arm32v7 AS installer

# Retrieve .NET Runtime
RUN dotnet_version=8.0.17 \
    && wget -O dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-musl-arm.tar.gz \
    && dotnet_sha512='3077b05a93a696dc34970d626dc69f08e173c354bbd979991404d79899c41ebf3fba9884d42a8f852e481ee85d8ca85eb2aefae31f8e4aeb2f7691d51e9df3ef' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:8.0.17-alpine3.21-arm32v7

# .NET Runtime version
ENV DOTNET_VERSION=8.0.17

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
