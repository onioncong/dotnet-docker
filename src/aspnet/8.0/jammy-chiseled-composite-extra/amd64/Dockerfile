ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM amd64/buildpack-deps:jammy-curl AS installer

# Retrieve ASP.NET Composite Runtime
RUN aspnetcore_version=8.0.17  \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-composite-$aspnetcore_version-linux-x64.tar.gz \
    && dotnet_sha512='2c36236bb66c6c2857fff0d1344bb056ac25f60997c1b5363f66ac9980d665804adfbd1c750aac3ea4fdc21186e14042f77303124708697eaae06a94ff1f6488' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz

RUN mkdir /dotnet-symlink \
    && ln -s /usr/share/dotnet/dotnet /dotnet-symlink/dotnet


# ASP.NET Composite Image
FROM $REPO:8.0.17-jammy-chiseled-extra-amd64

ENV \
    # .NET Runtime version
    DOTNET_VERSION=8.0.17 \
    # ASP.NET Core version
    ASPNET_VERSION=8.0.17

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/dotnet-symlink", "/usr/bin"]

ENTRYPOINT ["/usr/bin/dotnet"]
CMD ["--info"]
