FROM node:0.12.4
MAINTAINER Krzysztof Skoracki <krzysztof.skoracki@unit9.com>

RUN npm install -g coffee-script yo generator-hubot && \
    npm install underscore && \
	useradd hubot -m

USER hubot

WORKDIR /home/hubot

ENV BOT_NAME "bot"
ENV BOT_OWNER "No owner specified"
ENV BOT_DESC "Hubot with slack adapter"

ENV EXTERNAL_SCRIPTS=hubot-diagnostics,hubot-help,hubot-google-images,hubot-google-translate,hubot-pugme,hubot-maps,hubot-rules,hubot-shipit

RUN yo hubot --owner="$BOT_OWNER" --name="$BOT_NAME" --description="$BOT_DESC" --defaults && \
	sed -i /heroku/d ./external-scripts.json && \
	sed -i /redis-brain/d ./external-scripts.json && \
	npm install hubot-scripts@2.17.2 hubot-slack@4.3.1

CMD node -e "console.log(JSON.stringify(require('underscore').map('$EXTERNAL_SCRIPTS'.split(','), function(s) { return s.split('@')[0]; })))" > external-scripts.json && \
	npm install $(node -e "console.log('$EXTERNAL_SCRIPTS'.split(',').join(' '))") && \
	bin/hubot -n $BOT_NAME -a slack
