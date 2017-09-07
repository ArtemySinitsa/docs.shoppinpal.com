### How to debug failing e2e tests on CircleCI using VNC



1. Click on "Rebuild with SSH"
2. SSH to the circleci Instance with following command

```
ssh -p <ssh_port> <instance_ip> -L 5901:localhost:5900
```

Where,

**5901** is your localhost port mapped to the **5900** port of CircleCI instance

Now Open VNC client and put below address

```
localhost:5901
```

upon asking for password enter `secret` as password.

and you will now be able to view the tests running in remote browser.

