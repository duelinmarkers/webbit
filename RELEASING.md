How to do a Release
================

Prerequisites
--------

Releases are made with Maven. You need the following software installed:

* Maven 3
* GPG (If you're on a Mac, install [GPGTools for OS X](http://www.gpgtools.org/installer.html))

You also need a sonatype account and a `~/.m2/settings.xml` file with your credentials. See [Sonatype OSS Maven Repositories](https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide#SonatypeOSSMavenRepositoryUsageGuide-4.MavenRepositories).

Snapshot Releases
--------

Make sure the POM file has a SNAPSHOT version.

    mvn clean deploy

Releases
--------

Make sure the POM file has a SNAPSHOT version. The release plugin will bump it during the release process.

    mvn release:clean
    mvn release:prepare
    mvn release:perform

When prompted for an SCM tag, do not accept the default, enter instead v0.1.0, or whatever number you're releasing. We use the **v** prefix according to [SemVer](http://semver.org).
When the Maven release completes, build and deploy the full jar (with jarjar embedded netty)

    git co v0.1.0 && \
    make clean && \
    make && \
    mvn gpg:sign-and-deploy-file \
    -Durl=https://oss.sonatype.org/service/local/staging/deploy/maven2/ \
    -DrepositoryId=sonatype-nexus-staging \
    -DpomFile=pom.xml \
    -Dfile=dist/webbit.jar \
    -Dclassifier=full && \
    git co master