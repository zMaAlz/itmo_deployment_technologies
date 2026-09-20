FROM httpd:2.4
COPY ./deb_1с /dist/deb_1с
RUN dpkg -i /dist/deb_1с/*.deb && rm -rf /dist/deb_1с
