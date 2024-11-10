---
title: Using the Makefile
---
# Intro

This document explains how to use the <SwmPath>[Makefile](Makefile)</SwmPath> in the root directory of the ansible-power-aix repository. It will cover the various targets defined in the <SwmPath>[Makefile](Makefile)</SwmPath> and their purposes.

<SwmSnippet path="/Makefile" line="26">

---

# Utility Targets

The utility targets section defines general-purpose commands that can be used for various tasks.

```
######################################################################################
# utility targets
######################################################################################
```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="30">

---

The <SwmToken path="Makefile" pos="30:4:4" line-data=".PHONY: help">`help`</SwmToken> target provides usage information for the <SwmPath>[Makefile](Makefile)</SwmPath>. Running <SwmToken path="Makefile" pos="32:8:8" line-data="	@echo &quot;usage: make &lt;target&gt;&quot;">`make`</SwmToken>` `<SwmToken path="Makefile" pos="30:4:4" line-data=".PHONY: help">`help`</SwmToken> will display a list of available targets and their descriptions.

```
.PHONY: help
help:
	@echo "usage: make <target>"
	@echo ""
	@echo "target:"
	@echo "install-requirements ANSIBLE_VERSION=<version> 	install all requirements"
	@echo "install-ansible ANSIBLE_VERSION=<version>	install ansible: 2.9, 3, or 4"
	@echo "install-ansible-devel-branch			install ansible development branch"
	@echo "install-sanity-test-requirements		install python modules needed to \
	run sanity testing"
	@echo "install-unit-test-requirements 			install python modules needed \
	run unit testing"
	@echo "lint 						lint ansible module and roles"         
	@echo "module-lint MODULE=<module path> 		lint ansible module"         
	@echo "role-lint ROLE=<role path> 			lint ansible role"         
	@echo "porting MODULE=<module path>			check if module is python3 ported"
	@echo "sanity-test MODULE=<module path>		run sanity test on the collections"
	@echo "unit-test TEST=<test path>			run unit test suite for the collection"
	@echo "clean						clean junk files"

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="50">

---

The <SwmToken path="Makefile" pos="50:4:4" line-data=".PHONY: clean">`clean`</SwmToken> target removes temporary and cache files generated during the build and test processes. This includes Python cache files and collections.

```
.PHONY: clean
clean:
	@rm -rf tests/unit/plugins/modules/__pycache__
	@rm -rf tests/unit/plugins/modules/common/__pycache__
	@rm -rf collections/ansible_collections
	@rm -rf plugins/modules/__pycache__

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="57">

---

The <SwmToken path="Makefile" pos="57:4:6" line-data=".PHONY: uninstall-pylint">`uninstall-pylint`</SwmToken> target uninstalls the pylint package using pip. This can be useful if you need to reinstall or upgrade pylint.

```
.PHONY: uninstall-pylint
uninstall-pylint:
	python -m pip uninstall --yes pylint

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="62">

---

# Installation Targets

The installation targets section defines commands for installing various dependencies required for the project.

```
# installation targets
######################################################################################
```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="65">

---

The <SwmToken path="Makefile" pos="65:4:6" line-data=".PHONY: install-requirements">`install-requirements`</SwmToken> target installs all necessary requirements, including Ansible and Python modules needed for sanity and unit testing. It also upgrades pip.

```
.PHONY: install-requirements
install-requirements: install-ansible install-sanity-test-requirements \
		install-unit-test-requirements
	python -m pip install --upgrade pip

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="70">

---

The <SwmToken path="Makefile" pos="70:4:6" line-data=".PHONY: install-ansible">`install-ansible`</SwmToken> target installs a specific version of Ansible if the <SwmToken path="Makefile" pos="73:2:2" line-data="ifdef ANSIBLE_VERSION">`ANSIBLE_VERSION`</SwmToken> variable is set. If not, it installs the latest version of Ansible.

```
.PHONY: install-ansible
install-ansible:
	python -m pip install --upgrade pip
ifdef ANSIBLE_VERSION
	python -m pip install ansible==$(ANSIBLE_VERSION).*
else
	python -m pip install ansible
endif
```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="79">

---

The <SwmToken path="Makefile" pos="79:4:10" line-data=".PHONY: install-ansible-devel-branch">`install-ansible-devel-branch`</SwmToken> target installs the development branch of Ansible from the GitHub repository.

```
.PHONY: install-ansible-devel-branch
install-ansible-devel-branch:
	python -m pip install --upgrade pip
	python -m pip install https://github.com/ansible/ansible/archive/devel.tar.gz \
	--disable-pip-version-check

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="85">

---

The <SwmToken path="Makefile" pos="85:4:10" line-data=".PHONY: install-sanity-test-requirements">`install-sanity-test-requirements`</SwmToken> target installs Python modules needed to run sanity tests, as specified in the <SwmPath>[tests/sanity/sanity.requirements](tests/sanity/sanity.requirements)</SwmPath> file.

```
.PHONY: install-sanity-test-requirements
install-sanity-test-requirements:
	python -m pip install -r tests/sanity/sanity.requirements

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="89">

---

The <SwmToken path="Makefile" pos="89:4:10" line-data=".PHONY: install-unit-test-requirements">`install-unit-test-requirements`</SwmToken> target installs Python modules needed to run unit tests, as specified in the <SwmPath>[tests/unit/unit.requirements](tests/unit/unit.requirements)</SwmPath> file.

```
.PHONY: install-unit-test-requirements
install-unit-test-requirements:
	python -m pip install -r tests/unit/unit.requirements

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="93">

---

The <SwmToken path="Makefile" pos="93:4:8" line-data=".PHONY: install-pylint-py3k">`install-pylint-py3k`</SwmToken> target uninstalls the current version of pylint and installs a specific version compatible with Python 3.

```
.PHONY: install-pylint-py3k
install-pylint-py3k: uninstall-pylint
	python -m pip install --upgrade pip
	python -m pip install pylint==2.10.*

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="99">

---

# Testing Targets

The testing targets section defines commands for running various tests on the project.

```
# testing targets
######################################################################################
```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="102">

---

The <SwmToken path="Makefile" pos="102:4:4" line-data=".PHONY: lint">`lint`</SwmToken> target runs both module and role linting to ensure code quality and adherence to coding standards.

```
.PHONY: lint
lint: module-lint role-lint

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="105">

---

The <SwmToken path="Makefile" pos="105:4:6" line-data=".PHONY: module-lint">`module-lint`</SwmToken> target runs sanity tests and linting on Ansible modules using <SwmToken path="Makefile" pos="107:1:3" line-data="	ansible-test sanity -v --color yes --truncate 0 --python $(PYTHON_VERSION) \">`ansible-test`</SwmToken>, <SwmToken path="Makefile" pos="109:1:1" line-data="	flake8 --ignore=E402,W503 --max-line-length=160 --exclude $(DEPRECATED) $(MODULE)">`flake8`</SwmToken>, and <SwmToken path="Makefile" pos="110:6:6" line-data="	python -m pycodestyle --ignore=E402,W503 --max-line-length=160 --exclude $(DEPRECATED) \">`pycodestyle`</SwmToken>.

```
.PHONY: module-lint
module-lint:
	ansible-test sanity -v --color yes --truncate 0 --python $(PYTHON_VERSION) \
 	--exclude $(DEPRECATED) --test pylint $(MODULE)
	flake8 --ignore=E402,W503 --max-line-length=160 --exclude $(DEPRECATED) $(MODULE)
	python -m pycodestyle --ignore=E402,W503 --max-line-length=160 --exclude $(DEPRECATED) \
		$(MODULE)

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="113">

---

The <SwmToken path="Makefile" pos="113:4:6" line-data=".PHONY: role-lint">`role-lint`</SwmToken> target runs linting on Ansible roles using <SwmToken path="Makefile" pos="115:1:3" line-data="	ansible-lint --force-color -f pep8 $(ROLE)">`ansible-lint`</SwmToken>.

```
.PHONY: role-lint
role-lint:
	ansible-lint --force-color -f pep8 $(ROLE)

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="117">

---

The <SwmToken path="Makefile" pos="117:4:4" line-data=".PHONY: porting">`porting`</SwmToken> target checks if the modules are compatible with Python 3 using <SwmToken path="Makefile" pos="119:6:9" line-data="	python -m pylint --py3k --output-format=colorized $(MODULE) $(VIOSHC_SCRIPT)">`pylint --py3k`</SwmToken>.

```
.PHONY: porting
porting:
	python -m pylint --py3k --output-format=colorized $(MODULE) $(VIOSHC_SCRIPT)

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="121">

---

The <SwmToken path="Makefile" pos="121:4:4" line-data=".PHONY: compile">`compile`</SwmToken> target runs sanity tests to compile the modules and check for syntax errors.

```
.PHONY: compile
compile:
	ansible-test sanity --test compile --python $(PYTHON_VERSION) $(MODULE)

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="125">

---

The <SwmToken path="Makefile" pos="125:4:6" line-data=".PHONY: sanity-test">`sanity-test`</SwmToken> target runs sanity tests on the collection using <SwmToken path="Makefile" pos="127:1:3" line-data="	ansible-test sanity -v --color yes --truncate 0 --python $(PYTHON_VERSION) \">`ansible-test`</SwmToken>.

```
.PHONY: sanity-test
sanity-test:
	ansible-test sanity -v --color yes --truncate 0 --python $(PYTHON_VERSION) \
		--exclude $(DEPRECATED) $(MODULE)

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="130">

---

The <SwmToken path="Makefile" pos="130:4:6" line-data=".PHONY: unit-test">`unit-test`</SwmToken> target runs unit tests on the collection using <SwmToken path="Makefile" pos="133:1:3" line-data="		ansible-test coverage erase; \">`ansible-test`</SwmToken> and generates a coverage report.

```
.PHONY: unit-test
unit-test:
	@if [ -d "tests/output/coverage" ]; then \
		ansible-test coverage erase; \
	fi
	ansible-test units -v --color yes --python $(PYTHON_VERSION) \
	--coverage $(TEST)
	
	ansible-test coverage report --omit $(TEST_OMIT) --include "$(MODULE)" --show-missing

```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
