FROM magwas/edemotest:prezenta

RUN git clone https://github.com/magwas/zenta.git
RUN cd zenta;mkdir ./org.rulez.magwas.zenta.integrationtests/generated-images;Xvnc :0&export DISPLAY=:0;mvn install
RUN rm -rf zenta

