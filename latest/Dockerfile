FROM debian:stretch

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

RUN groupadd -r chromium && useradd -r -g chromium -G audio,video -m chromium
RUN chown -R chromium:chromium /home/chromium/

USER chromium

EXPOSE 9222

ENTRYPOINT [\
  "chromium",\
  "--headless",\
  "--disable-gpu",\
  "--no-sandbox",\
  "--disable-software-rasterizer",\
  "--disable-background-networking",\
  "--enable-features=NetworkService,NetworkServiceInProcess",\
  "--disable-background-timer-throttling",\
  "--disable-backgrounding-occluded-windows",\
  "--disable-breakpad",\
  "--disable-client-side-phishing-detection",\
  "--disable-component-extensions-with-background-pages",\
  "--disable-default-apps",\
  "--disable-dev-shm-usage",\
  "--disable-extensions",\
  "--disable-features=TranslateUI,BlinkGenPropertyTrees",\
  "--disable-hang-monitor",\
  "--disable-ipc-flooding-protection",\
  "--disable-popup-blocking",\
  "--disable-prompt-on-repost",\
  "--disable-renderer-backgrounding",\
  "--disable-sync",\
  "--force-color-profile=srgb",\
  "--metrics-recording-only",\
  "--no-first-run",\
  "--enable-automation",\
  "--password-store=basic",\
  "--use-mock-keychain",\
  "--remote-debugging-port=9222",\
  "--remote-debugging-address=0.0.0.0"]
