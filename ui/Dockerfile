## Multi stage Angular docker file

FROM node:10.24.1 AS angular_stage
WORKDIR /app
COPY package.json package-lock.json /app
RUN npm install 

COPY . /app
RUN npm run build 

FROM nginx:latest

## This will copy browser ready static files into ngnix main configuaration file which is "/usr/share/nginx/html" folder.
## Using the 
COPY --from=angular_stage /app/dist/pillar/ /usr/share/nginx/html
EXPOSE 80



