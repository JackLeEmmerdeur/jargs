JArgs command-line argument parsing library
===========================================

- Copyright (c) 2001-2012 Steve Purcell.
- Copyright (c) 2002      Vidar Holen.
- Copyright (c) 2002      Michal Ceresna.
- Copyright (c) 2005      Ewan Mellor.
- Copyright (c) 2010-2012 penSec.IT UG (haftungsbeschränkt).

All rights reserved.

Released under the terms of the BSD licence.  See the file `LICENCE` for
details.


Prerequisites
-------------

For each prerequisite, the version with which JArgs has been tested is given
in parentheses.  Any version equal to or later than this should work.

To build JArgs and run its tests you need on of

- [Apache Ant](http://ant.apache.org/) (1.8.2), by The Apache Software
  Foundation
- [Apache Maven](http://maven.apache.org/) (3.0.4), by The Apache Software
  Foundation

Moreover [JUnit](http://www.junit.org/) (4.3.1), by Eric Gamma, is used to run
the unit tests, but is not needed to run the library itself.


Installation
------------

To compile, package, and test the code, run either

    ant

or

    mvn clean package source:jar javadoc:jar jar:test-jar

Two jars are created, one called `target/jargs-$VERSION$.jar`, which contains
the runtime library, and one called `target/jargs-$VERSION$-tests.jar`, which
contains the unit tests and the examples.  The Javadoc APIs are created in
`target/site/apidocs`.

To use the library with your own code, simply ensure that
`target/jargs-$VERSION$.jar` is on the CLASSPATH.

Pre-built packages are available via Sonatype's OSS Snapshots maven
repository. Ensure you have the oss-public repository enabled in your
`pom.xml`:

```xml
<repository>
  <id>sonatype-oss-public</id>
  <url>https://oss.sonatype.org/content/groups/public/</url>
  <releases>
    <enabled>true</enabled>
  </releases>
  <snapshots>
    <enabled>true</enabled>
  </snapshots>
</repository>
```

Then add JArgs to your `dependencies` list tag:

```xml
<dependency>
  <groupId>com.sanityinc</groupId>
  <artifactId>jargs</artifactId>
  <version>2.0-SNAPSHOT</version>
</dependency>
```

Documentation
-------------

The main documentation is the detailed worked example in
`src/examples/java/com/sanityinc/jargs/examples/OptionTest.java`, plus the
generated API documentation in `target/site/apidocs`.


Message from the fork-maintainer
--------------------------------
I know the proposed solution for generating automatic parameter- and usage-help
is hackish atm. I hope to continue work on it soon.

Another option would be, to use the source from a different fork made by mwnorman,
to be found under [mwnorman/jargs](https://github.com/mwnorman/jargs).
I believe he did something similar. I spotted it to late when my code was already
finished, but his code looks a little more sophisticated.

The fork defines some new methods on the CmdLineParser- and Option-classes,
where some of them are overloaded. You'll have to look into the sources
API-Doc, or follow the howto below, to decipher their usages.

<ins>CmdLineParser</ins>:
```
setHelptextExample(String helptextExampleHeader, String helptextExampleBody)
setHelptextIntro(String helptextIntroHeader, String helptextIntroBody)
gethelptext(boolean drawDeco, boolean returnedCachedHelptext, String helptextParameterHeader)
gethelptext (
	boolean drawDeco,
	String decolineBold,
	String decolineThin,
	boolean returnedCachedHelptext,
	String helptextParameterHeader
)
```
<ins>Option</ins>:
```
setHelptext(String helptext)
setHelptext(String[] helptextrows)
setHelptext(String valueCaption, String helptext)
setHelptext(String valueCaption, String[] helptextrows)
```


<b>Howto</b>:

After you've initialized your parser and added some options like this...
```
CmdLineParser parser = new CmdLineParser();
Option<String> optionAppMode = parser.addStringOption('a',"appmode");
Option<Boolean> optionKillSwitch = parser.addBooleanOption('k',"killswitch");
```
...you're ready to add some valuable pieces of blabbering with...
```
parser.setHelptextIntro("MyApp Version 0.0.1", "Doing this\r\nFailing at that");
parser.setHelptextExample("Example:", "java -jar MyApp.jar --appmode=FAIL");
```
With setHelptextIntro() you set a title and a descriptive text.
With setHelptextExample()...Maybe you've guessed it already.
```
optionKillSwitch.setHelptext("Believe me: leave this option as is"); 
optionAppMode.setHelptext("MODE",
  new String[] {
    "Sets modus operandi of the app",
    "where MODE could be either",
    "\tDO",
    "\tor",
    "\tFAIL"
  });
System.out.println(parser.gethelptext(true, true, "Command line options:"));
```
The setHelptext-Method accepts one or two arguments, where the first (here MODE)
sets a descriptive Valuecaption for a long-commandline-option and will be printed
like this in the helptext:
-a, --appmode=MODE
If your option doesn't contain the long-form just provide the helptext.

The second argument for setHelptext is the descriptive helptext for the option,
and can either be a single line in the form of a string, or multiple lines
using a string array. Excuse my dim witted try at formatting. It works for me.

parser.gethelptext(...) will eventually generate the helptext. The method is
overloaded with a different signature, for providing user defined symbols
for printing the decoration around the initial helptext block. Look into
the API-Documentation within the source for additional help.



Package contents
----------------

- `src/main/java/com/sanityinc/jargs` -- The library itself.
- `src/examples/java/com/sanityinc/jargs/examples` -- Examples showing how to
  use the library.
- `src/test/java/com/sanityinc/jargs` -- JUnit tests.
- `target/site/apidocs` -- API and other documentation.
- `target/classes` -- Compiled classes, once built.
- `target/` -- JArgs jars, once built.

<hr>

[![](http://api.coderwall.com/purcell/endorsecount.png)](http://coderwall.com/purcell)

[![](http://www.linkedin.com/img/webpromo/btn_liprofile_blue_80x15.png)](http://uk.linkedin.com/in/stevepurcell)

[Steve Purcell's blog](http://www.sanityinc.com/) // [@sanityinc on Twitter](https://twitter.com/sanityinc)

