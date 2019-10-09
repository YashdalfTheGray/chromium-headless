# Word of caution - this should not be used for any reason.
# This image was built as an experiment and to learn some
# more nginx configuration options.
FROM nginx:latest

COPY nginx.conf /etc/nginx/nginx.conf

RUN apt-get update && apt-get install -y --no-install-recommends \
  curl \
  chromium \
  fonts-liberation \
  fonts-roboto \
  libcanberra-gtk-module \
  libexif-dev \
  libgl1-mesa-dri \
  libgl1-mesa-glx \
  libpango1.0-0 \
  libv4l-0 \
  fonts-symbola

EXPOSE 8080

COPY init.sh /usr/bin/init.sh

ENTRYPOINT ["sh", "-c"]
CMD ["/usr/bin/init.sh"]
