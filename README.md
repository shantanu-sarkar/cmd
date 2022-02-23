## Force sync ROM
```bash
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```
## Initial Alt cache partition commands
```bash
sudo mkdir /mnt/ccache
```
```bash
ccache -M 50G -F 0
```
# For AOSP Extended
## Build user + installclean + Alt cache partition
```bash
source build/envsetup.sh
lunch aosp_guacamoleb-user
sudo mount --bind ~/.cache /mnt/ccache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/ccache
make installclean
export WITH_GAPPS=false
m aex -j$(nproc --all) | tee log.txt
```
## Build user + installclean + Alt cache partition + GAPPS
```bash
source build/envsetup.sh
lunch aosp_guacamoleb-user
sudo mount --bind ~/.cache /mnt/ccache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/ccache
make installclean
export WITH_GAPPS=true
m aex -j$(nproc --all) | tee log.txt
```
# For LineageOS
## installclean + ALt cache partition
```bash
source build/envsetup.sh
breakfast guacamoleb
sudo mount --bind ~/.cache /mnt/ccache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/ccache
make installclean
croot
brunch guacamoleb
```
## Clone YAAP HALs
```bash
rm -rf hardware/qcom-caf/sm8150/display
git clone https://github.com/yaap/hardware_qcom-caf_sm8150_display hardware/qcom-caf/sm8150/display/
rm -rf hardware/qcom-caf/sm8150/audio
git clone https://github.com/yaap/hardware_qcom-caf_sm8150_audio hardware/qcom-caf/sm8150/audio/
rm -rf hardware/qcom-caf/sm8150/media
git clone https://github.com/yaap/hardware_qcom-caf_sm8150_media hardware/qcom-caf/sm8150/media/
```
## Gerrit sample command for AEX
For pushing to gerrit
```bash
git push ssh://shantanu-sarkar@gerrit.aospextended.com:29418/AospExtended/* HEAD:refs/for/12.x
```
For hook
```bash
gitdir=$(git rev-parse --git-dir); scp -p -P 29418 shantanu-sarkar@gerrit.aospextended.com:hooks/commit-msg ${gitdir}/hooks/
git commit
```
