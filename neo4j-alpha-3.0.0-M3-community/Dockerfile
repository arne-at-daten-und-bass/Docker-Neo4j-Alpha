###__________________________________________________###
### DOCKERfile for neo4j 3.0.0 alpha nigthly release ###
### Tested on OSX with Docker Toolbox Version 1.9.1  ###
### Tested with neo4j 3.0.0-M03 on 2015-01-21        ###


# Experimental neo4j image based on nightly releases from:
#	- http://alpha.neohq.net
#
# This dockerfile and docker-entrypoint.sh are based on the neo4j/neo4j files for version 2.3.1 from:
#	- https://hub.docker.com/r/neo4j/neo4j/
# 	- https://github.com/neo4j/docker-neo4j/tree/master/2.3.1
# 
# The Dockerfile from there has been adjusted to the nightly builds:
# 	- Changes are marked as 
# 		- "Changed value(s)", 
#		- "Removed" or 
#		- "Added"
#
# The docker-entrypoint.sh from there has not been changed, yet it had to be explicitly set "executable" here
#	- RUN chmod +x /docker-entrypoint.sh (see below)


##_____________##
## DOCKERfile: ##

FROM java:openjdk-8-jre

RUN apt-get update --quiet --quiet \
    && apt-get install --quiet --quiet --no-install-recommends lsof \
    && rm -rf /var/lib/apt/lists/*
# Changed value:
ENV NEO4J_VERSION 3.0.0
ENV NEO4J_EDITION community
# Removed: 
# ENV NEO4J_DOWNLOAD_SHA256 7b2f30d73af107eacd9a3a619475ef448a08eecb2cdb42ab1f8a38d091c70ecb
# Changed values:
ENV NEO4J_DOWNLOAD_ROOT http://alpha.neohq.net/dist
ENV NEO4J_TARBALL neo4j-$NEO4J_EDITION-$NEO4J_VERSION-NIGHTLY-unix.tar.gz
ENV NEO4J_URI $NEO4J_DOWNLOAD_ROOT/$NEO4J_TARBALL

RUN echo $NEO4J_URI

RUN curl --fail --silent --show-error --location --output neo4j.tar.gz $NEO4J_URI \
	# Removed:
	# && echo "$NEO4J_DOWNLOAD_SHA256 neo4j.tar.gz" | sha256sum --check --quiet - \
    && tar --extract --file neo4j.tar.gz --directory /var/lib \
    && mv /var/lib/neo4j-* /var/lib/neo4j \
    && rm neo4j.tar.gz


WORKDIR /var/lib/neo4j

RUN mv data /data \
    && ln --symbolic /data

VOLUME /data

COPY docker-entrypoint.sh /docker-entrypoint.sh
# Added:
RUN chmod +x /docker-entrypoint.sh

EXPOSE 7474 7473

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["neo4j"]


##________________________________##
## DOCKER: Image instructions ##
#
# $ cd .
# $ docker build -t neo_alpha_3_0_0_img .
# Optional:
# 	- $ docker images


##________________________________##
## DOCKER: Container instructions ##
#
# $ docker run -itd -p 7474:7474  --name neo_alpha_3_0_0_ctr neo_alpha_3_0_0_img
# 	
# Optional:
# 	- To have a local copy of the data directory, specify the -v flag:
# 		- $ docker run <...> -v /path/to/your/local/data/directoy:/data <...>
# 	- $ docker logs neo_alpha_3_0_0_ctr
# 		- Exemplary result: 
# 			Starting Neo4j Server.
# 			INFO  Successfully started database
#			INFO  Starting HTTP on port 7474 (1 threads available)
# 			INFO  Enabling HTTPS on port 7473
#			INFO  Mounting static content at /webadmin
#			INFO  Mounting static content at /browser
#			INFO  Remote interface ready and available at http://0.0.0.0:7474/
# 	- § docker exec -it neo_alpha_3_0_0_ctr bash


##__________________________##
## OSX: Browser instruction ##
#
# In a browser on the local machine (hosting Docker Toolbox):
#	- http://<docker-machine-ip-default>:7474 
#		- $ docker-machine ip default
#	- Tested on OSX with Docker Toolbox and Safari & Chrome

# Have fun!

