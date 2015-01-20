PROJECT = DNA_HomeScreen
INSTALL_FILES = images js icon.png index.html
WRT_FILES = DNA_common css icon.png index.html setup config.xml images js manifest.json
VERSION := 0.0.1
PACKAGE = $(PROJECT)-$(VERSION)

SEND := ~/send

ifndef TIZEN_IP
#TIZEN_IP=TizenNuc
TIZEN_IP=192.168.1.103
endif

wgtPkg: clean
	cp -rf ../DNA_common .
	zip -r $(PROJECT).wgt config.xml css icon.png index.html js images DNA_common
	
config:
	scp setup/weston.ini root@$(TIZEN_IP):/etc/xdg/weston/

$(PROJECT).wgt : dev

wgt:
	zip -r $(PROJECT).wgt $(WRT_FILES)

run: install
	ssh app@$(TIZEN_IP) "export DBUS_SESSION_BUS_ADDRESS='unix:path=/run/user/5000/dbus/user_bus_socket' && xwalkctl | egrep -e 'Home Screen' | awk '{print $1}' | xargs --no-run-if-empty xwalk-launcher -d "

install: deploy
	ssh app@$(TIZEN_IP) "export DBUS_SESSION_BUS_ADDRESS='unix:path=/run/user/5000/dbus/user_bus_socket' && xwalkctl | egrep -e 'Home Screen' | awk '{print $1}' | xargs --no-run-if-empty xwalkctl -u"
	ssh app@$(TIZEN_IP) "export DBUS_SESSION_BUS_ADDRESS='unix:path=/run/user/5000/dbus/user_bus_socket' && xwalkctl -i /home/app/DNA_HomeScreen.wgt"

$(PROJECT).wgt : wgt

deploy: wgtPkg
	scp $(PROJECT).wgt app@$(TIZEN_IP):/home/app

all:
	@echo "Nothing to build"


clean:
	-rm $(PROJECT).wgt
	-rm -rf DNA_common