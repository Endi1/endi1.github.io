# Why password-store is the best password manager

In my never-ending quest in improving the various day-to-day tools that I use, there's one particular tool that I have never found the need to replace. Strangely enough, it's also the tool I probably use the most. I'm talking about [password-store](https://www.passwordstore.org/).

Password-store (or pass) is one of the best (if not _the_ best) password managers out there. If you have never heard of it, here's a quick intro for you.

_pass_ is a simple but powerful password manager that fully adheres to the UNIX philosophy of doing one thing and doing it well. It's mainly a CLI tool but it also has several UI clients available as well.

Pass works by saving individual user passwords as GPG-encrypted files inside folders named as the site they belong to. So for example, if you have an account named foo for a site bar the folder structure would be:


    ~/.password-store/

        bar/

            foo.gpg


What's great is that this folder can then be version controlled (usually with git) and there's even built-in integration with git. So you just add a remote git repository when setting up pass for the first time and then every time you want to back up your passwords you simply run pass git push. This way you have full control over your passwords and you avoid relying on third-party services for which you have no absolute safety on how your passwords are stored or whether they will keep staying in business.

_pass_ also generates new passwords for you with varying degrees of strength and length. So for example if you want to generate a 64 character long password for the aforementioned bar website you simply run pass generate _bar/foo 64_. To copy that password to your clipboard you run _pass -c bar/foo_. That's it. Those are probably the only commands you need to use with pass

There is also a free and open source pass [client for Android](https://f-droid.org/packages/com.zeapo.pwdstore/) that can be used along [OpenKeychain](https://f-droid.org/packages/org.sufficientlysecure.keychain/) for sharing and managing your private PGP key(s).