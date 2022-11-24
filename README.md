# ClassicPress-Coding-Standards

Proposed ClassicPress coding standards for reviewing plugins/themes, presented as a WPCS phpcs.xml ruleset.

## Installation

These instructions assume the use of Linux, OS X or Windows Subsystem for Linux. PRs that describe how to run the sniffs on Windows are welcome but this is likely to be more difficult.

1. Create a directory dedicated to this project (e.g. `/home/username/cp-plugin-review`)
2. Change to the directory (`cd /home/username/cp-plugin-review`)
3. Install WPCS v2.3.0: `composer create-project wp-coding-standards/wpcs wpcs 2.3.0 --no-dev --prefer-dist --keep-vcs` ([more info](https://github.com/WordPress/WordPress-Coding-Standards#installation)). Now, WPCS should be installed into the `wpcs` subdirectory.
4. Clone this repository (`git clone https://github.com/ClassicPress/ClassicPress-Coding-Standards`).

## Usage

1. Change back to your plugin-review directory (`cd /home/username/cp-plugin-review`)
2. Clone the plugin you want to analyze (`git clone https://github.com/azurecurve/azrcrv-redirect`).
3. Change to the plugin's directory (`cd azrcrv-redirect`).
4. Make note of the plugin's textdomain in the main PHP file (in this case, `azrcrv-r`).
5. Run the `bin/cpcs` script from this repository as follows: `CPCS_TEXTDOMAIN=azrcrv-r ../ClassicPress-Coding-Standards/bin/cpcs .`

If you wish to show all warnings/errors even if they have been suppressed by `// phpcs:ignore` comments in the code, then add `--ignore-annotations` to the end of the above command line.

This will produce an error log like this example:
```
FILE: path/to/file.php
---------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 11 ERRORS AND 4 WARNINGS AFFECTING 11 LINES
---------------------------------------------------------------------------------------------------------------------------------------------------
  30 | ERROR   | eval() is a security risk so not allowed.
 100 | ERROR   | Missing $domain arg.
 200 | ERROR   | All output should be run through an escaping function
     |         | '$var'.
 450 | WARNING | Processing form data without nonce verification.

---------------------------------------------------------------------------------------------------------------------------------------------------
```

The Standard also includes a (by default not active) PHP Compatibility Sniff.
To use that, uncomment this section in the XML file:
```
<!-- <config name="testVersion" value="5.2-"/>
<rule ref="PHPCompatibility">
    <include-pattern>*\.php$</include-pattern>
</rule> -->
```
Then make sure you have `PHPCompatibility` installed (if not, [these steps](https://github.com/PHPCompatibility/PHPCompatibility#installation-via-a-git-check-out-to-an-arbitrary-directory-method-2) work great).

The error report will now include by default also PHP Compatibility Details.
To change the minimum and manximum supported PHP version, amend the line `<config name="testVersion" value="5.2-"/>` as you prefer, for example `<config name="testVersion" value="5.5-7.4"/>`

## Principles

- only security related issues should be flagged with the exception:
    - text domain is passed to localisation functions
    - text domain passed in function matches declared domain
- no styling, formatting or documentation issues should be flagged
- should not sniff CSS or JS files. Should sniff _only_ PHP Files.
- should check for a minimum supported _WP_ Version
- should not be considered a final judgement - instead a human being should evaluate the results and work on a solution with the developer

This ruleset represents an attempt of satisfying everyone and still have some standards.

Public discussion that lead to this ruleset can be found [here](https://forums.classicpress.net/t/adopt-wpcs-for-themes-and-plugin-directory/3755/)

These Coding standards are aimed towards Theme and Plugin _reviewers_.
Developers are still encouraged to actually use a more thorough Standard ([WPCS `WordPress` standard](https://github.com/WordPress/WordPress-Coding-Standards) would be highly recommended).
The ruleset should help when revieweing plugins for security issues and not serve as a referece of what quality code looks like.

This rulest is _NOT_ to be used for CP Core either.

While crafting code compliant with this ruleset _will_ make it easier for your plugin to be listed on the CP Directory, it does _not_ guarantee it, since there are many other conditions to be fullfilled.

Again, this is NOT a "good" coding standard to improve your CodeBase in general. 
It would be better to use the more complete WPCS, and if you follow that, it will pass this Ruleset without issues.

## Contributors

To contribute, you will need to know the exact rule you want to add or remove so to add it to the XML file.

The rule names appear in the log like this:
```
FILE: /Users/bedaschmid/Desktop/vars-master/classes/UpdateClient.class.php
---------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AFFECTING 1 LINE
---------------------------------------------------------------------------------------------------------------------------------------------------
 100 | ERROR   | Missing $domain arg. (WordPress.WP.I18n.MissingArgDomain)
...
---------------------------------------------------------------------------------------------------------------------------------------------------
```
`WordPress.WP.I18n.MissingArgDomain` is the rule throwing the error.

Consult the existing rules in the XML file and the related rule documentations to see how to remove, add, configure it more granularly.
