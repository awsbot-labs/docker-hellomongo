FROM tomcat:latest

RUN apt-get update && apt-get install -y maven git

RUN git clone https://github.com/dcrbsltd/hellomongo.git
RUN cd hellomongo && mvn clean install && cd -

RUN rm -rf webapps/ROOT/* && mv hellomongo/target/hellomongo/* webapps/ROOT/

RUN apt-get remove -y maven git && apt-get autoremove -y && apt-get autoclean -y

