# This docker is based on ubuntu 16.04
# The objective is to run sen2cor in this docker
FROM ubuntu:xenial

LABEL maintainer "Lufuno Vhengani <lvhengani@csir.co.za>"

ENV DEBIAN_FRONTEND noninteractive

ARG SEN2COR_VERSION='02.08.00'
ARG SEN2COR_DIRECTORY='2.8.0' 
ARG GOSU_VERSION='1.10'
ARG SEN2COR="Sen2Cor-${SEN2COR_VERSION}-Linux64.run"
RUN apt-get update && apt-get install -y \
        unzip \
        curl \
        file \
        wget && \
    rm -rf /var/lib/apt/lists/* && \
    wget "http://step.esa.int/thirdparties/sen2cor/${SEN2COR_DIRECTORY}/${SEN2COR}" && \
    bash ${SEN2COR} && \
    rm ${SEN2COR} && \
    cp -p /Sen2Cor-${SEN2COR_VERSION}-Linux64/lib/python2.7/site-packages/sen2cor/cfg/L2A_GIPP.xml /root/sen2cor/2.8/cfg/L2A_GIPP_without_dem.xml && \
    sed -n 's/<DEM_Directory>NONE<\/DEM_Directory>/<DEM_Directory>dem<\/DEM_Directory>/;p' /root/sen2cor/2.8/cfg/L2A_GIPP_without_dem.xml > /root/sen2cor/2.8/cfg/L2A_GIPP_with_dem.xml && \
    sed -i 's/<DEM_Directory>NONE<\/DEM_Directory>/<DEM_Directory>dem<\/DEM_Directory>/' /root/sen2cor/2.8/cfg/L2A_GIPP_with_dem.xml &&\
    sed -i 's/<DEM_Reference>NONE<\/DEM_Reference>/<DEM_Reference>http:\/\/data_public:GDdci@data.cgiar-csi.org\/srtm\/tiles\/GeoTIFF\/<\/DEM_Reference>/' /root/sen2cor/2.8/cfg/L2A_GIPP_with_dem.xml && \
    sed -i 's/<Aerosol_Type>RURAL<\/Aerosol_Type>/<Aerosol_Type>AUTO<\/Aerosol_Type>/' /root/sen2cor/2.8/cfg/L2A_GIPP_with_dem.xml &&\
    sed -i 's/<Mid_Latitude>SUMMER<\/Mid_Latitude>/<Mid_Latitude>AUTO<\/Mid_Latitude>/' /root/sen2cor/2.8/cfg/L2A_GIPP_with_dem.xml && \
    sed -i 's/<Cirrus_Correction>FALSE<\/Cirrus_Correction>/<Cirrus_Correction>TRUE<\/Cirrus_Correction>/' /root/sen2cor/2.8/cfg/L2A_GIPP_with_dem.xml && \
    sed -i 's/<Downsample_20_to_60>TRUE<\/Downsample_20_to_60>/<Downsample_20_to_60>FALSE<\/Downsample_20_to_60>/' /root/sen2cor/2.8/cfg/L2A_GIPP_with_dem.xml && \
    curl -o /usr/local/bin/gosu -sSL "https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-amd64" && \
    chmod +x /usr/local/bin/gosu
ADD docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh
