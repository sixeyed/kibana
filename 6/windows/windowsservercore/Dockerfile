# escape=`
FROM mcr.microsoft.com/windows/servercore:ltsc2019
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Kibana
ENV KIBANA_VERSION="6.5.3" 
#ENV KIBANA_SHA1="9A5A7B23F99C1A334F3E891860480AA587654550"

#RUN Invoke-WebRequest -OutFile kibana.zip "https://artifacts.elastic.co/downloads/kibana/kibana-$($env:KIBANA_VERSION)-windows-x86_x64.zip" -UseBasicParsing; `
#    if ((Get-FileHash kibana.zip -Algorithm sha1).Hash -ne $env:KIBANA_SHA1) {exit 1} ; `
#    Expand-Archive kibana.zip -DestinationPath C:\ ; `
#    Move-Item c:\kibana-$($env:KIBANA_VERSION)-windows-x86_64 'c:\kibana'; `
#    Remove-Item kibana.zip

RUN Invoke-WebRequest -OutFile kibana.zip "https://artifacts.elastic.co/downloads/kibana/kibana-$($env:KIBANA_VERSION)-windows-x86_64.zip" -UseBasicParsing;
RUN Expand-Archive kibana.zip -DestinationPath C:\ ;
RUN Move-Item c:/kibana-$($env:KIBANA_VERSION)-windows-x86_64 'c:\kibana';
RUN Remove-Item kibana.zip

# Default configuration for host & Elasticsearch URL
WORKDIR c:/kibana
RUN (Get-Content ./config/kibana.yml) -replace '#server.host: \"localhost\"', 'server.host: \"0.0.0.0\"' | Set-Content ./config/kibana.yml; `
    (Get-Content ./config/kibana.yml) -replace '#elasticsearch.url: \"http://localhost:9200\"', 'elasticsearch.url: \"http://elasticsearch:9200\"' | Set-Content ./config/kibana.yml

# REMARKS - DNS tweaks needed for Windows
# RUN Set-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ServerPriorityTimeLimit -Value 0 -Type DWord    

EXPOSE 5601
COPY docker-entrypoint.ps1 /
ENTRYPOINT ["powershell"]
CMD ["/docker-entrypoint.ps1"]
