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
brunch guacamoleb | tee log.txt
```
# For Pixel Experience
## Build userdebug + installclean + Alt cache partition
```bash
source build/envsetup.sh
lunch aosp_guacamoleb-userdebug
sudo mount --bind ~/.cache /mnt/ccache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/ccache
make installclean
mka bacon -j$(nproc --all) | tee log.txt
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
## Sign all commits by default (Windows)
```bash
git config user.signingkey 5D570797CF704721
git config commit.gpgsign true
```
## Sign all commits by default (WSL)
```bash
git config user.signingkey 7D85D86F7CA9A82D
git config commit.gpgsign true
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
## Gerrit sample command for LOS
For pushing to gerrit
```bash
git push ssh://shantanu-sarkar@review.lineageos.org:29418/LineageOS/* HEAD:refs/for/lineage-19.1
```
For hook
```bash
gitdir=$(git rev-parse --git-dir); scp -p -P 29418 shantanu-sarkar@review.lineageos.org:hooks/commit-msg ${gitdir}/hooks/
```
## Gerrit sample command for PE
For pushing to gerrit
```bash
git push ssh://shantanu-sarkar@gerrit.pixelexperience.org:29418/* HEAD:refs/for/twelve-plus
```
For hook
```bash
gitdir=$(git rev-parse --git-dir); scp -p -P 29418 shantanu-sarkar@gerrit.pixelexperience.org:hooks/commit-msg ${gitdir}/hooks/
```
No edit
```bash
git commit --amend --no-edit
```
