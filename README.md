# Android jenkins setup example
This project shows our jenkins setup. It contains:

* Lint

![Lint](/wiki/images/lint.png)

* Checkstyle

![Checkstyle](/wiki/images/checkstyle.PNG)

* FindBugs

![FindBugs](/wiki/images/findbugs.PNG)

* PMD

![PMD](/wiki/images/pmd.PNG)

* Duplicate code

![Duplicate code](/wiki/images/duplicate_code.PNG)

* Open Tasks (TODO, FIXME,..)

![Open Tasks](/wiki/images/open_tasks.PNG)

* Static Analysis

![Static Analysis](/wiki/images/static_analysis.PNG)

* Code lines count (*.java, *xml)

![Lines count](/wiki/images/sloc.PNG)

* Lines delta per commit

![Lines delta](/wiki/images/lines_delta.PNG)

* Code coverage

![Code coverage](/wiki/images/code_coverage.PNG)

* Tests Result

![Tests Result](/wiki/images/test_results.PNG)

# Jenkins configuration

These plugins should be installed in Jenkins:
* Android Lint Plugin - This plugin parses Android Lint analysis results and visualises the issues found.
* Checkstyle Plug-in - This plug-in collects the Checkstyle analysis results of the project modules and visualizes the found warnings.
* Duplicate Code Scanner Plug-in - This plug-in collects the duplicate code analysis results of the project modules and visualizes the found warnings.
* FindBugs Plug-in - This plug-in collects the FindBugs analysis results of the project modules and visualizes the found warnings.
* JaCoCo plugin - This plugin allows you to capture code coverage report from JaCoCo. Jenkins will generate the trend report of coverage. This plugin is fork of the [Emma Plugin]. Big part of the code structure comes from it, however, it is completely refactored. It also includes functionality similar to the [Emma Coverage Column] which allows to include a column in Dashboards which displays the latest overall coverage numbers and links to the coverage report.
* JUnit Plugin - Allows JUnit-format test results to be published.
* PMD Plug-in - This plug-in collects the PMD analysis results of the project modules and visualizes the found warnings.
* SLOCCount Plug-in - This plugin parses SLOCCount output files to produce project and build reports.
* Static Analysis Utilities - This plug-in provides utilities for the static code analysis plug-ins.
* Task Scanner Plug-in - This plug-in scans for open tasks in a specified set of files in the project modules and visualizes the results.

To get lines count You have to install Cloc (or Sloc) tool. You can download cloc from [here](http://cloc.sourceforge.net/).


**Project configurations:**
![Build](/wiki/images/build.PNG)
![Lint_checkstyle](/wiki/images/lint_checkstyle.PNG)
![findbugs_pmd_duplicate_code](/wiki/images/findbugs_pmd_duplicate_code.PNG)
![Tasks](/wiki/images/tasks.PNG)
![JUnit_sloc_jacoco](/wiki/images/junit_sloc_jacoco.PNG)


* "Build" -> "Invoke Gradle script" tasks should contains these lines
```
clean
assemble
check
```


* After that, add "Execute shell" with command:
```
cloc --by-file --xml --out=app/build/reports/cloc.xml app/src/main
```


**In "Post-build Actions":**
* Add "Publish Android Lint results" with Lint files:
```
**/lint-results*.xml
```


* Add "Publish Checkstyle analysis results" with Checkstyle results:
```
**/app/build/reports/checkstyle/*.xml
```


* Add "Publish FindBugs analysis results" with FindBugs results:
```
**/app/build/reports/findbugs/*.xml
```


* Add "Publish PMD analysis results" with PMD results:
```
**/app/build/reports/pmd/*.xml
```


* Add "Publish duplicate code analysis results" with PMD results:
```
**/app/build/reports/cpd/*.xml
```


* Add "Scan workspace for open tasks" with:


Files to scan:
```
**/*.java, **/*.xml
```
High priority:
```
FIXME
```
Normal priority:
```
TODO
```


* Add "Publish JUnit test result report" with Test report XMLs:
```
**/build/test-results/**/TEST-*.xml
```


* Add "Publish SLOCCount analysis results" with SLOCCount reports:
```
app/build/reports/cloc.xml
```


* Add "Record JaCoCo coverage report" with:

Path to exec files (e.g.: **/jacoco.exec):
```
**/**.exec
```

Path to class directories (e.g.: **/target /classDir, **/classes):
```
**/classes/debug
```

Path to source directories (e.g.: **/mySourceFiles):
```
**/src/main/java
```

Exclusions (e.g.: **/*Test*.class):
```
**/R*, **/R$.class, **/*$ViewBinder*.*, **/*$ViewInjector*.*, **/BuildConfig.*, **/Manifest*.*
```

# Android project configuration

To enable project stats reporting, You have to add few lines to module build.gradle:
```groovy
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'de.aaschmid.gradle.plugins:gradle-cpd-plugin:0.4'
    }
}
```

and 

```groovy
apply plugin: 'com.android.application'
apply from: 'config/quality.gradle'
```

Also copy app/config folder to your module. If your module name is not "app" - You should replace all "app" titles in configs to match Yours. 




License
=======

    Copyright 2015 Telesoftas.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
