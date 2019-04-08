ARG PERL_VERSION

ARG CUSTOM_AWSCLI_IMAGE
ARG CUSTOM_AWSCLI_VERSION

FROM perl:${PERL_VERSION} AS builder

COPY /templates/ /tmp/tpl/

ARG S3_ACCESS_KEY
ARG S3_SECRET_KEY
ARG S3_REGION

RUN mkdir /aws \
 && cat /tmp/tpl/config.ini | \
    S3_REGION="${S3_REGION}" \
    perl -p \
    -e 's/\Q{{S3_REGION}}\E/$ENV{S3_REGION}/g;' \
    > /aws/config \
 && cat /tmp/tpl/credentials.ini | \
    S3_ACCESS_KEY="${S3_ACCESS_KEY}" \
    S3_SECRET_KEY="${S3_SECRET_KEY}" \
    perl -p \
    -e 's/\Q{{S3_ACCESS_KEY}}\E/$ENV{S3_ACCESS_KEY}/g;' \
    -e 's/\Q{{S3_SECRET_KEY}}\E/$ENV{S3_SECRET_KEY}/g;' \
    > /aws/credentials

FROM ${CUSTOM_AWSCLI_IMAGE}:${CUSTOM_AWSCLI_VERSION} AS base

COPY --from=builder /aws/ /root/.aws/
