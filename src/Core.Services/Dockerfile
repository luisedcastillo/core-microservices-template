#FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build-env
#WORKDIR /app
#
## Copy csproj and restore as distinct layers
#COPY *.csproj ./
#RUN dotnet restore
#
## Copy everything else and build
#COPY . ./
#RUN dotnet publish -c Release -o out

#https://docs.newrelic.com/docs/agents/net-agent/installation/install-docker-container

FROM microsoft/aspnetcore:2.0
FROM microsoft/dotnet:2.0.0-sdk-2.0.2-stretch

# Install the agent
RUN apt-get update && apt-get install -y wget ca-certificates gnupg \
&& echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
&& wget https://download.newrelic.com/548C16BF.gpg \
&& apt-key add 548C16BF.gpg \
&& apt-get update \
&& apt-get install -y newrelic-netcore20-agent

ENV CORECLR_ENABLE_PROFILING="1" \
CORECLR_PROFILER="{36032161-FFC0-4B61-B559-F6C5D41BAE5A}" \
CORECLR_NEWRELIC_HOME="/usr/local/newrelic-netcore20-agent" \
CORECLR_PROFILER_PATH="/usr/local/newrelic-netcore20-agent/libNewRelicProfiler.so" \
NEW_RELIC_LICENSE_KEY="3746113fec7f21e85135087aa6d47225369db05e" \
NEW_RELIC_APP_NAME="My New APP"


WORKDIR /app

ENV ASPNETCORE_URLS http://+:8601
EXPOSE 8601
COPY ${source:-obj/Docker/publish} .
ENTRYPOINT ["dotnet", "Core.Services.dll"]
