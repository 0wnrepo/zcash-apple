# Yet Another Zcash Builder for Apple Platform

![Screenshot](https://github.com/kozyilmaz/zcash-apple/raw/master/docs/zcash-apple.png "Zcash on Mac OS")

This repository builds standalone Zcash binaries for macOS platform without installing brew.  
No additional dependency required except Xcode (https://developer.apple.com/xcode).  
All build tools (`autoconf, automake, libtool, pkgconfig, cmake, install and readlink`) and `Zcash` are compiled from scratch (finally with `clang`).  


### Build instructions

`$ git clone https://github.com/kozyilmaz/zcash-apple.git`  
`$ cd zcash-apple`  
`$ source environment`  
`$ make`

After successful build Zcash binaries will be installed to `out` directory under project root  
You can then copy binary directory anywhere you like there are no dependencies to the build tree anymore  
```
bash-3.2$ ls -lrt out/usr/local/bin
total 31544
-rwxr-xr-x  1 loki  staff       483 Dec 16 15:56 zcash-init
-rwxr-xr-x  1 loki  staff  13120252 Dec 20 01:06 zcashd
-rwxr-xr-x  1 loki  staff   1772400 Dec 20 01:06 zcash-tx
-rwxr-xr-x  1 loki  staff      4761 Dec 20 01:06 zcash-fetch-params
-rwxr-xr-x  1 loki  staff   1237116 Dec 20 01:06 zcash-cli
bash-3.2$ otool -L out/usr/local/bin/zcashd
out/usr/local/bin/zcashd:
    /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1238.60.2)
    /usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 307.5.0)
bash-3.2$ otool -L out/usr/local/bin/zcash-cli 
out/usr/local/bin/zcash-cli:
    /usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 307.5.0)
    /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1238.60.2)
bash-3.2$ otool -L out/usr/local/bin/zcash-tx
out/usr/local/bin/zcash-tx:
    /usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 307.5.0)
    /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1238.60.2)
```

### Run instructions

When launching `Zcash` on MacOS for the first time, certain initalization steps should be completed.  
Please run the commands below once for the first time  

`$ cd out/usr/local/bin`  
`$ ./zcash-fetch-params`  
`$ ./zcash-init`  
`$ ./zcashd`  

You can just run `Zcash` by launching the daemon afterwards  

`$ ./zcashd`  

Console output from the first run is below:
```
bash-3.2$ ./zcash-fetch-params
Zcash - fetch-params.sh

This script will fetch the Zcash zkSNARK parameters and verify their
integrity with sha256sum.

If they already exist locally, it will exit now and do nothing else.
The parameters are currently just under 911MB in size, so plan accordingly
for your bandwidth constraints. If the files are already present and
have the correct sha256sum, no networking is used.

Creating params directory. For details about this directory, see:
/Users/loki/Library/Application Support/ZcashParams/README

Retrieving: https://z.cash/downloads/sprout-proving.key
######################################################################## 100.0%
/Users/loki/Library/Application Support/ZcashParams/sprout-proving.key.dl: OK
/Users/loki/Library/Application Support/ZcashParams/sprout-proving.key.dl -> /Users/loki/Library/Application Support/ZcashParams/sprout-proving.key
Retrieving: https://z.cash/downloads/sprout-verifying.key
######################################################################## 100.0%
/Users/loki/Library/Application Support/ZcashParams/sprout-verifying.key.dl: OK
/Users/loki/Library/Application Support/ZcashParams/sprout-verifying.key.dl -> /Users/loki/Library/Application Support/ZcashParams/sprout-verifying.key
bash-3.2$ 
bash-3.2$ cat zcash-init 
#!/bin/bash

# excerpted from zclassic/zutil/init-mac.sh

if [ ! -f "$HOME/Library/Application Support/Zcash/zcash.conf" ]; then
    echo "Creating zcash.conf"
    mkdir -p "$HOME/Library/Application Support/Zcash/"
    echo "rpcuser=zcashrpc" > ~/Library/Application\ Support/Zcash/zcash.conf
    PASSWORD=$(cat /dev/urandom | env LC_CTYPE=C tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1)
    echo "rpcpassword=$PASSWORD" >> "$HOME/Library/Application Support/Zcash/zcash.conf"
    echo "Complete!"
fi
bash-3.2$ 
bash-3.2$ ./zcash-init 
Creating zcash.conf
Complete!
```

Vaklinov's [Desktop GUI Wallet](https://github.com/vaklinov/zcash-swing-wallet-ui) also works, please follow build instructions for MacOS


## Thanks
Developers of `Zcash`  
Developers of `ZClassic` for MacOS patches

## Donations
If you feel this project is useful to you. Feel free to donate.

    BTC address: 1GmXRm5sEATy3Kz1hCxS1dwfXuCPkevsa
    ZEC address: t1MW8Vx4SF1ewmL3rTN8UfRxULFTaugh1ab


### Disclaimer
This program is not officially endorsed by or associated with the Zcash project and the Zcash company.
[Zcash®](https://trademarks.justia.com/871/93/zcash-87193130.html) and the 
[Zcash® logo](https://trademarks.justia.com/868/84/z-86884549.html) are trademarks of the
[Zerocoin Electric Coin Company](https://trademarks.justia.com/owners/zerocoin-electric-coin-company-3232749/).

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

