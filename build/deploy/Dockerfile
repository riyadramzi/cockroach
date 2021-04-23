FROM registry.access.redhat.com/ubi8/ubi-minimal

# For deployment, we need the following additionally installed:
# tzdata - for time zone functions; reinstalled to replace the missing
#          files in /usr/share/zoneinfo/
# hostname - used in cockroach k8s manifests
# tar - used by kubectl cp

RUN whoami
RUN pwd
RUN microdnf update -y \
    && rpm --erase --nodeps tzdata \
    && microdnf install tzdata hostname tar -y \
    && rm -rf /var/cache/yum

RUN mkdir /usr/local/lib/cockroach 
RUN mkdir /cockroach 
RUN mkdir /licenses
RUN ls -la
RUN whoami
COPY cockroach.sh cockroach /cockroach/
COPY licenses/* /licenses/
# Install GEOS libraries.
COPY libgeos.so libgeos_c.so /usr/local/lib/cockroach/

# Set working directory so that relative paths
# are resolved appropriately when passed as args.
RUN pwd
WORKDIR /cockroach/
RUN pwd
# Include the directory in the path to make it easier to invoke
# commands via Docker
ENV PATH=/cockroach:$PATH

ENV COCKROACH_CHANNEL=official-docker

EXPOSE 26257 8080
ENTRYPOINT ["/cockroach/cockroach.sh"]
