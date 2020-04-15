### H3lix 32位 uicache failed fix

#### Can't install H3lix because uicache failed, looked it up says you need to sign without the "get-task-allow" entitlement

### 参考地址<https://github.com/pixelomer/AltDeploy/issues/30>

```
Download the patch.sh
Extract it.
Copy it the patch.sh to the desktop
Copy the iPA to the desktop
Open a terminal and run cd ~/Desktop
Run chmod +x patch.sh
Run bash patch.sh h3lix-RC6.ipa h3lix-RC6-patch.ipa
Follow this to sign it
Jailbreak.
```
#### patch.sh 文件备份

```
if [ $# != 2 ]; then
    echo "Usage: $0 /path/to/input_ipa /path/to/output_ipa"
    exit 1
fi

if ! [ -f $1 ]; then
    echo "'$1' does not exist"
    exit 1
fi

if [ -f $2 ]; then
    echo "'$2' already exists"
    exit 1
fi

echo "Setting up environment"
mkdir /tmp/unpacked_h3lix
if [ $? != 0 ]; then
    echo "mkdir create temporary directory"
    exit 1
fi

echo "Extracting"
unzip $1 -d /tmp/unpacked_h3lix > /dev/null
if [ $? != 0 ]; then
    echo "can't unzip '$1'"
    rm -rf /tmp/unpacked_h3lix
    exit 1
fi

echo "Patching"

# tada tada P\WX+1y~~z??ti.....
(printf '\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11' | dd of=/tmp/unpacked_h3lix/Payload/h3lix.app/h3lix bs=1 seek=30848 count=20 conv=notrunc) 2> /dev/null
(printf '\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11' | dd of=/tmp/unpacked_h3lix/Payload/h3lix.app/h3lix bs=1 seek=32920 count=20 conv=notrunc) 2> /dev/null

# i DoN'T hAz CS_GET_TASK_ALLOW?!?!??
(printf '\x00\x00\x00\x00' | dd of=/tmp/unpacked_h3lix/Payload/h3lix.app/h3lix bs=1 seek=31790 count=4 conv=notrunc) 2> /dev/null

# DeBuG Br34K
(printf '\x70\x47' | dd of=/tmp/unpacked_h3lix/Payload/h3lix.app/h3lix bs=1 seek=40800 count=2 conv=notrunc) 2> /dev/null

echo "Compressing"

CD=$(pwd)
cd /tmp/unpacked_h3lix

if [[ "$2" = /* ]]; then
    zip -r $2 Payload/ > /dev/null
else
    zip -r "$CD/$2" Payload/ > /dev/null
fi

if [ $? != 0 ]; then
    echo "can't zip '$1'"
    rm -rf /tmp/unpacked_h3lix
    cd - > /dev/null
    exit 1
fi

cd - > /dev/null
rm -rf /tmp/unpacked_h3lix
echo "Done"
exit 0
```