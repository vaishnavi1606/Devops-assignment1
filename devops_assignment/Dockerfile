FROM node:dubnium
ENV NODE_OPTIONS --max-old-space-size=2048
WORKDIR /app
COPY ./package* ./
RUN ls -al
RUN npm install && \
    npm cache clean --force
COPY . .
EXPOSE 51005
CMD ["node", "index.js"]

