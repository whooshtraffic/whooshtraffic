all: html commit push

html:
	sphinx-build -b html ./source ./

commit:
	git commit -a -m "Building new version of docs"

push:
	git push
