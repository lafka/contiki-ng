all: coap-example-server coap-example-client
# use target "plugtest-server" explicitly when required

CONTIKI=../..

-include $(CONTIKI)/Makefile.identify-target

# automatically build RESTful resources
REST_RESOURCES_DIR = ./resources
ifeq ($(TARGET), native)
 REST_RESOURCES_FILES = $(notdir $(shell find $(REST_RESOURCES_DIR) -name '*.c'))
else
 REST_RESOURCES_FILES = $(notdir $(shell find $(REST_RESOURCES_DIR) -name '*.c' ! -name 'res-plugtest*'))
endif

PROJECTDIRS += $(REST_RESOURCES_DIR)
PROJECT_SOURCEFILES += $(REST_RESOURCES_FILES)

# Include the CoAP implementation
MODULES += os/net/app-layer/coap

include $(CONTIKI)/Makefile.include

# border router rules
$(CONTIKI)/tools/tunslip6:	$(CONTIKI)/tools/tunslip6.c
	(cd $(CONTIKI)/tools && $(MAKE) tunslip6)

connect-router:	$(CONTIKI)/tools/tunslip6
	sudo $(CONTIKI)/tools/tunslip6 fd00::1/64

connect-router-cooja:	$(CONTIKI)/tools/tunslip6
	sudo $(CONTIKI)/tools/tunslip6 -a 127.0.0.1 -p 60001 fd00::1/64

connect-router-native:	$(CONTIKI)/examples/rpl-border-router/border-router.native
	sudo $(CONTIKI)/examples/rpl-border-router/border-router.native -a 127.0.0.1 -p 60001 fd00::1/64

connect-native:
	sudo ip address add fdfd::1/64 dev tun0
