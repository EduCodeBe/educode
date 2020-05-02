#NOM	: Makefile
#HOWTO	: make, make crowdin-pull, make crowdin-push, make deploy, make deploy-test
#AUTEUR	: 
#CONTACT: 

# The Crowdin's api key 
CROWDIN_API_KEY:=$(shell grep -oP "crowdinApiKey=\K\w+" Secret)
# Crowdin's project id
CROWDIN_PROJECT_ID:=educode
# File's name of translated string from Crowdin
CROWDIN_TRANSLATIONS_FILE:=translated.zip
# Username of the ftp acces for the server
SERVER_USERNAME:=$(shell grep -oP "serverUsername=\K\w+" Secret)
# Password of the ftp acces for the server
SERVER_PWD:=$(shell grep -oP "serverPwd=\K\w+" Secret)

# Install Lektor, i18n plugin for Lektor and 'zip/unzip' to unzip the translations from Crowdin
setup : install-lektor install-i18n install-crowdin

# Install Lektor
install-lektor :
	@echo Installing Lektor
	sudo apt install python-dev libssl-dev libffi-dev python-pip imagemagick
	pip install Lektor

# Install Lektor i18n plugin for the project
install-i18n : 
	@echo Installing Lektor i18n plugin
	sudo apt install gettext python3-babel
	pip install lektor-i18n

# Allow to use 'make crawdin-push' and 'make crowdin-pull'
install-crowdin :
	@echo Installing zip for Crowdin
	sudo apt install zip

# Extract the strings and put them on Crowdin
crowdin-push : i18n/contents.pot
	lektor build 
	@echo Pushing the translations
	curl -F "files[/educode2020.pot]=@i18n/contents.pot" https://api.crowdin.com/api/project/${CROWDIN_PROJECT_ID}/update-file?key=${CROWDIN_API_KEY}

# Get the translated string from Crowdin and put them into the project
crowdin-pull :
	@echo Trying to build and download the translations
	curl -f https://api.crowdin.com/api/project/${CROWDIN_PROJECT_ID}/export?key=${CROWDIN_API_KEY}
	curl -f https://api.crowdin.com/api/project/${CROWDIN_PROJECT_ID}/download/all.zip?key=${CROWDIN_API_KEY} --output ${CROWDIN_TRANSLATIONS_FILE}
	@echo Trying to unzip the translations
	unzip -j -o ${CROWDIN_TRANSLATIONS_FILE} 2020/* -d i18n/ || exit 1
	rm ${CROWDIN_TRANSLATIONS_FILE}
	@echo Building the project to apply the translations
	lektor build 

# Deploy the site on the main domain
deploy :
	lektor build
	lektor deploy --username ${SERVER_USERNAME} --password ${SERVER_PWD}

# Deploy the site on the test domain
deploy-test :
	lektor build
	lektor deploy test --username ${SERVER_USERNAME} --password ${SERVER_PWD}