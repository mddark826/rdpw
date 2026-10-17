FROM debian:bookworm-slim
ENV DEBIAN_FRONTEND=noninteractive
ENV WINEDEBUG=-all
ENV WINEDLLOVERRIDES="mscoree,mshtml="
RUN dpkg --add-architecture i386 \
 && apt-get update && apt-get install -y --no-install-recommends \
    xvfb x11vnc novnc websockify fluxbox xterm firefox-esr ca-certificates wget \
    wine wine64 wine32:i386 \
 && rm -rf /var/lib/apt/lists/*
RUN mkdir -p /root/.vnc && x11vnc -storepasswd sadik123 /root/.vnc/passwd \
 && ln -sf /usr/share/novnc/vnc.html /usr/share/novnc/index.html
RUN printf '#!/bin/bash\nexport DISPLAY=:0\nXvfb :0 -screen 0 1024x600x16 &\nsleep 2\nfluxbox &\nx11vnc -display :0 -rfbauth /root/.vnc/passwd -forever -shared -rfbport 5900 -localhost &\nexec websockify --web=/usr/share/novnc ${PORT:-10000} localhost:5900\n' > /start.sh \
 && chmod +x /start.sh
EXPOSE 10000
CMD ["/start.sh"]
