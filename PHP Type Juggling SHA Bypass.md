The main idea finding the flag is exploiting PHP type juggling.
#### Step-1:

After I visited the URL: [https://webctf.cloudsek.com/the-sha-juggler](https://webctf.cloudsek.com/the-sha-juggler), the page was mostly empty.

Upon inspecting HTML code, I found a `const` defined in `<script>` tag but not used anywhere. It had hex encoded string.

After hex decoding the string, I was greeted with below code:

```php
<?php
// you_found_me.php
if (isset($_GET['hash'])) {
    if ($_GET['hash'] === "10932435112") {
        die('Do you think its that easy??');
    }
    $hash = sha1($_GET['hash']);
    $target = sha1(10932435112);
    if($hash == $target) {
        include('flag.php');
        print $flag;
    } else {
        print "CSEK{n0_4lag_4_u}";
    }
} 
?>
```


#### Step-2:

On inspecting the PHP code, it was clear a weak algorithm (sha1) is used for calculating the hash.

I setup php repl on my machine. Got the Sha1 of `10932435112` is `0e07766915004133176347055865026311692244`.

The comparison `if($hash == $target)` is vulnerable because it is not a strict comparison with `===` as stated in this [stackoverflow thread](https://security.stackexchange.com/questions/268218/crashing-the-sha1-function-in-php).

> The baseline is that, == operator in PHP converts strings which look like a number to a number before comparing.

So, `sha(10932435112) gives 0e07766915004133176347055865026311692244`, which in integer terms is `0*10^07766915004133176347055865026311692244`.

We know that `==` converts anything which looks like integer, so 0^anthing is zero. Now this value is getting compared to the `$hash` variable which is the sha1(`$hash` which we send).

Example:

```
'0e123' == '0e345' is true
'0e123' === '0e345' is false
```

> So we need to find a string whose sha1() produces a hash starting with 0e`

Any other hashes, will give false flag of `CSEK{n0_4lag_4_u}`.
#### Step-3:

I just googled “sha1 hash starting with 0e”. I used this [link](https://github.com/spaze/hashes/blob/master/sha1.md).

So, I tried the URL as any of the below. All have to work because all de-reference to same hash.

- aaK1STfY
- aaroZmOk
- aaO8zKZF
- aa3OFF9m

Got the actual flag with sending this: https://webctf.cloudsek.com/the-sha-juggler/you_found_me.php?hash=aaK1STfY

Voila! I got the flag.
