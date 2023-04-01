FROM python:3.9.2-alpine3.13 as build
WORKDIR /wheels
RUN apk add --no-cache \
    ncurses-dev \
    build-base
COPY docker_reqs.txt /opt/OxtraTools/requirements.txt
RUN pip3 wheel -r /opt/OxtraTools/requirements.txt


FROM python:3.9.2-alpine3.13
WORKDIR /home/OxtraTools
RUN adduser -D OxtraTools

COPY --from=build /wheels /wheels
COPY --chown=OxtraTools:OxtraTools requirements.txt /home/OxtraTools/
RUN pip3 install -r requirements.txt -f /wheels \
  && rm -rf /wheels \
  && rm -rf /root/.cache/pip/* \
  && rm requirements.txt

COPY --chown=OxtraTools:OxtraTools src/ /home/OxtraTools/src
COPY --chown=osintgram:OxtraTools main.py /home/OxtraTools/
COPY --chown=osintgram:OxtraToolsconfig/ /home/OxtraTools/config
USER OxtraTools

ENTRYPOINT ["python", "main.py"]
