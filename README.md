    NAME
        markafile - file tagging for *nix operating systems

    DESCRIPTION
        markafile is a utility program for tagging files and searching files by
        tags using a powerful search expression.

        Tags are case in-sensitive, may not contain space(s). Any number of tags
        may be applied to any file. File tags are stored in an SQLite database.

        Should any error occur, markafile will exit with status 1 and print error
        details to STDERR.

    REQUIRED PERMISSIONS
        markafile requires the following permissions:
        - Read-Write permission on SQLite database file
        - Read-Write permission on SQLite database file directory (1)
        - Read-Execute permission on tagged directory (2)
        - Read permission on tagged file

        (1) markafile creates the database file if it does not exist.

        (2) markafile tags directory files, it does not tag the directory itself.

    TAG FILES
        To add tags to a file:
        $ markafile tag "startup console conf" /etc/motd

        To add tags to multiple files:
        $ markafile tag "conf cron script" /etc/cron.daily /etc/cron.hourly

        To add tags to directory files:
        $ markafile tag "script" /etc/rc.d

        To add tags to directory files (including sub-directories)
        $ markafile tag "srv-conf conf" /etc/httpd /etc/ppp -r

        File tag may not:
        - be "and" "or" "not" "(" ")"
        - contain space(s)
        - contain asterisk(s)

    SEARCH FILES
        To print all tagged files and tags under a directory:
        $ markafile find "*" /etc/httpd

        To print all tagged files and tags under a directory (including
        sub-directories):
        $ markafile find "*" /etc -r

    COMPLEX SEARCH
        Search expression is a mini-language consists of tags, keywords and
        parenthesis. The expression is translated into SQL query for execution.

        TAG NAMES
            Case in-sensitive file tags
        KEYWORDS
            Logical operators "and", "or", "not"
        PARANTHESIS
            To elevate precedence of their enclosed expression

        Simple search expression example:
        $ markafile find "srv-conf" /etc -r

        Complex example:
        $ markafile find "conf and srv-conf and not (cron or script)" /etc -r

    REMOVE FILE TAGS
        Use action "untag", specify search expression followed by ":" and tags to
        be removed:
        $ markafile untag "not (cron or script):srv-conf conf" /etc -r

        If ":*" follows the search expression, all tags will be removed from search
        result:
        $ markafile untag "not (cron or script):*" /etc -r

        To remove all tags from all files:
        $ markafile untag "*:*" /etc -r

    BUGS
        When tagged files are moved/deleted, their original paths will remain in
        database thus causing incorrect path-tag associations. However, search
        results are guaranteed to only return results of correct and valid paths.

    AUTHOR
        Howard Guo <guohouzuo@gmail.com>

    REPORTING BUGS
        Please contact the author by Email for bug report and feedback.

    COPYRIGHT
        Copyright (c) 2012 Howard Guo
        All rights reserved.

        Redistribution and use in source and binary forms, with or without
        modification, are permitted provided that the following conditions are met:

        1. Redistributions of source code must retain the above copyright notice,
          this list of conditions and the following disclaimer.
        2. Redistributions in binary form must reproduce the above copyright
          notice, this list of conditions and the following disclaimer in the
          documentation and/or other materials provided with the distribution.

        THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
        AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
        IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
        ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE
        LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
        CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
        SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
        INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
        CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
        ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
        POSSIBILITY OF SUCH DAMAGE.