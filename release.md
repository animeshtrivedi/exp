## Configure your environment for a release 

## Preapring for a release 
A release consists of a doing a (i) source release; (b) binary release; (iii) uploading maven artifacts; (iv) updating documentation. To do a version release of `x.y`, follow these steps: 
  1. Go through the closed JIRAs and merge requests, and update the HISTORY.md file about what is new in the new release version. 
  2. Perform `mvn apache-rat:check` 
  3. Perform `mvn stylecheck:check` 
  4. `mvn release:prepare -P apache-release -Darguments="-DskipTests"  -DinteractiveMode=true -Dresume=false` 
     The interactive mode allows us to explicitly name the current release version, release candidate, and next version. The convention here is to follow `apache-crail-x.y-incubating-rcX` naming, starting from release candidate 0. How to do another release candidate is disucssed later. 
  5. Now we need to rename the artifacts to follow the naming convention  
  
  6. Generate checksum files for source and binary files
  ```
  sha512sum apache-crail-x.y-incubating-src.tar.gz > apache-crail-x.y-incubating-src.tar.gz.sha512
  sha512sum apache-crail-x.y-incubating-bin.tar.gz > apache-crail-x.y-incubating-bin.tar.gz.sha512
  ```

Step 5 and 6 will be automated once the [JIRA-56](https://issues.apache.org/jira/projects/CRAIL/issues/CRAIL-56) is fixed.
  
  7. Verify checksums for source and binary files 
  ```
  sha512sum -c apache-crail-x.y-incubating-src.tar.gz.sha512
  sha512sum -c apache-crail-x.y-incubating-bin.tar.gz.sha512
  ```
  
  8. Verify signtures for source and binary files 
  ```
  gpg --verify apache-crail-x.y-incubating-src.tar.gz.asc apache-crail-x.y-incubating-src.tar.gz
  gpg --verify apache-crail-x.y-incubating-bin.tar.gz.asc apache-crail-x.y-incubating-bin.tar.gz
  ```
  
  9. We need to upload the generated artifacts to SVN staging at `https://dist.apache.org/repos/dist/dev/incubator/crail/`  
  

## Voting 
```
Subject: [VOTE] Release of Apache Crail-${RELEASE_VERSION}-incubating [rcX]
============================================================================

Hi all,

This is a call for a vote on releasing Apache Crail ${RELEASE_VERSION}-incubating, release candidate X.

The source and binary tarball, including signatures, digests, etc. can be found at:
https://dist.apache.org/repos/dist/dev/incubator/crail/${RELEASE_VERSION}-incubating-rcX/

The commit to be voted upon:
https://git-wip-us.apache.org/repos/asf?p=incubator-crail.git;a=commit;h=[REF]

The Nexus Staging URL:
https://repository.apache.org/content/repositories/orgapachecrail-[STAGE_ID]

Release artifacts are signed with the following key:
https://people.apache.org/keys/committer/<apache_user_name>.asc

For information about the contents of this release, see:
https://git-wip-us.apache.org/repos/asf?p=incubator-crail.git;a=blob;f=HISTORY.md;h=33626ffd22abb751a8a69f503476197678aa4128;hb=49951523cac723f5793ff3971fab190920ae6745
or https://github.com/apache/incubator-crail/blob/v${RELEASE_VERSION}-rcX/HISTORY.md

Please vote on releasing this package as Apache Crail ${RELEASE_VERSION}-incubating

The vote will be open for 72 hours.

[ ] +1 Release this package as Apache Crail ${RELEASE_VERSION}-incubating
[ ] +0 no opinion
[ ] -1 Do not release this package because ...


Thanks,
[YOUR_NAME]
```

### Convention to preapre for another release condiate 

## After acceptance 
  * Tag the commit with the release version without -rcX"
  * Upload to the release SVN https://dist.apache.org/repos/dist/release/incubator
  * Write a resulting email and announce on the mailing list 
  * Update the download page on the website 
  * Social media (Twitter, LinkedIn announcement)  

## Useful links
  * General info for release signing: https://www.apache.org/dev/release-signing.html
  * http://tephra.incubator.apache.org/ReleaseGuide.html
  * https://dubbo.incubator.apache.org/en-us/blog/prepare-an-apache-release.html 
