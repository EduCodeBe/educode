#NOM	: Makefile
#HOWTO	: 
#AUTEUR	: 
#CONTACT: 

CROWDIN_API_KEY:=$(shell cat CrowdinApiKey)
CROWDIN_PROJECT_ID:=educode
CROWDIN_TRANSLATIONS_FILE:=translated.zip

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

crowdin-push : i18n/contents.pot
	@echo Pushing the translations
	curl -F "files[/educode2020.pot]=@i18n/contents.pot" https://api.crowdin.com/api/project/${CROWDIN_PROJECT_ID}/update-file?key=${CROWDIN_API_KEY}

crowdin-pull :
	@echo Trying to build and download the translations
	curl -f https://api.crowdin.com/api/project/${CROWDIN_PROJECT_ID}/export?key=${CROWDIN_API_KEY}
	curl -f https://api.crowdin.com/api/project/${CROWDIN_PROJECT_ID}/download/all.zip?key=${CROWDIN_API_KEY} --output ${CROWDIN_TRANSLATIONS_FILE}
	@echo Trying to unzip the translations
	unzip -j -o ${CROWDIN_TRANSLATIONS_FILE} 2020/* -d i18n/ || exit 1
	rm ${CROWDIN_TRANSLATIONS_FILE}
	@echo Building the project to apply the translations
	lektor build 