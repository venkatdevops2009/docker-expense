FROM node:20
# creates /app and set the directory to /app
WORKDIR /app
COPY package.json .
COPY *.js .
RUN npm install
ENV DB_HOST=mysql \
    DB_USER=expense \
    DB_PWD=ExpenseApp@1 \
    DB_DATABASE=transactions    
CMD ["node", "index.js"]
