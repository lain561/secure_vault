# what we use wolfi for in here:
# Minimal base images, the use of a non-root user, 
# and a focus on only including necessary components.
#We also use volumes to hold the files even when the container is not running

FROM cgr.dev/chainguard/wolfi-base

ARG version=3.12
WORKDIR /app

# Define the volume, this will allow us to store the files even after the container stops running
VOLUME ["/app/Vuploads"]

RUN apk add python-${version} py${version}-pip && \
	chown -R nonroot.nonroot /app/

# Copy the SSL certificates
COPY certificate.pem /app/
COPY private_key.pem /app/

#minimum permissions to run
USER nonroot

#copy the requirements that are only needed 
COPY requirements.txt data.py /app/

#uploads directory
RUN mkdir uploads

#copying templates
COPY templates /app/templates/
COPY templates /app/templates/subFolder/

#copying CSS
COPY static/css /app/static/css/
COPY static/images /app/static/images/
COPY static/css/fonts /app/static/css/fonts/

#health check
HEALTHCHECK CMD curl --fail http://localhost:8080/health || exit 1

#only install the files needed
RUN  pip install -r requirements.txt --user

#settings a defined entry point
ENTRYPOINT [ "python", "data.py" ]
