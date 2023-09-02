## Force sync ROM
```bash
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```
# For Pixel Experience
## Dirty Build
```bash
source build/envsetup.sh
lunch aosp_guacamoleb-userdebug
sudo mount --bind ~/.cache /mnt/cache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/cache
mka bacon -j$(nproc --all) | tee log.txt
```
## Installclean
```bash
source build/envsetup.sh
make installclean
lunch aosp_guacamoleb-userdebug
sudo mount --bind ~/.cache /mnt/cache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/cache
mka bacon -j$(nproc --all) | tee log.txt
```
## For Kernel
```bash
source build/envsetup.sh
lunch aosp_guacamoleb-userdebug
sudo mount --bind ~/.cache /mnt/cache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/cache
make bootimage -j$(nproc --all) | tee log.txt
```
# For LineageOS
## Dirty build
```bash
source build/envsetup.sh
breakfast guacamoleb
sudo mount --bind ~/.cache /mnt/cache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/cache
croot
brunch guacamoleb | tee log.txt
```
## Installclean
```bash
source build/envsetup.sh
make installclean
breakfast guacamoleb
sudo mount --bind ~/.cache /mnt/cache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/cache
croot
brunch guacamoleb | tee log.txt
```
## For Kernel
```bash
source build/envsetup.sh
breakfast guacamoleb
sudo mount --bind ~/.cache /mnt/cache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/cache
croot
make bootimage -j$(nproc --all) | tee log.txt
```
## Gerrit sample command for LOS
For pushing to gerrit
```bash
git push ssh://shantanu-sarkar@review.lineageos.org:29418/LineageOS/* HEAD:refs/for/lineage-20
```
For hook
```bash
gitdir=$(git rev-parse --git-dir); scp -p -P 29418 shantanu-sarkar@review.lineageos.org:hooks/commit-msg ${gitdir}/hooks/
```
```bash
git commit --amend --no-edit
```
## Gerrit sample command for PE
For pushing to gerrit
```bash
git push ssh://shantanu-sarkar@gerrit.pixelexperience.org:29418/* HEAD:refs/for/thirteen-plus
```
For hook
```bash
gitdir=$(git rev-parse --git-dir); scp -p -P 29418 shantanu-sarkar@gerrit.pixelexperience.org:hooks/commit-msg ${gitdir}/hooks/
```
```bash
git commit --amend --no-edit
```
## Gerrit LOS setup
```
git config --global user.email 'shantanuplussarkar@gmail.com'
git config --global review.review.lineageos.org.username "shantanu-sarkar"
```
## Gerrit PE setup
```
git config --global user.email 'shantanuplussarkar@gmail.com'
git config --global review.gerrit.pixelexperience.org.username "shantanu-sarkar"
```
## Initial Alt cache partition commands
```bash
sudo mkdir /mnt/cache
```
```bash
ccache -M 75G -F 0
```
## Merge Tags for msm-4.14
```bash
git remote add fw-api https://git.codelinaro.org/clo/la/platform/vendor/qcom-opensource/wlan/fw-api
git fetch fw-api <TAG>
git merge -X subtree=drivers/staging/fw-api FETCH_HEAD

git remote add qca-wifi-host-cmn https://git.codelinaro.org/clo/la/platform/vendor/qcom-opensource/wlan/qca-wifi-host-cmn
git fetch qca-wifi-host-cmn <TAG>
git merge -X subtree=drivers/staging/qca-wifi-host-cmn FETCH_HEAD

git remote add qcacld-3.0 https://git.codelinaro.org/clo/la/platform/vendor/qcom-opensource/wlan/qcacld-3.0
git fetch qcacld-3.0 <TAG>
git merge -X subtree=drivers/staging/qcacld-3.0 FETCH_HEAD

git remote add data https://git.codelinaro.org/clo/la/platform/vendor/qcom-opensource/data-kernel
git fetch data <TAG>
git merge -X subtree=techpack/data FETCH_HEAD

git remote add audio https://git.codelinaro.org/clo/la/platform/vendor/opensource/audio-kernel
git fetch audio <TAG>
git merge -X subtree=techpack/audio FETCH_HEAD

git fetch https://git.codelinaro.org/clo/la/kernel/msm-4.14 <TAG>
git merge FETCH_HEAD

git fetch https://android.googlesource.com/kernel/common/ upstream-f2fs-stable-linux-4.14.y
git merge FETCH_HEAD

git fetch https://android.googlesource.com/kernel/common/ android-4.14-stable
git merge FETCH_HEAD
```
## First time
```bash
git remote add fw-api https://git.codelinaro.org/clo/la/platform/vendor/qcom-opensource/wlan/fw-api
git fetch fw-api <TAG>
git merge -s ours --no-commit --allow-unrelated-histories FETCH_HEAD
git read-tree --prefix=drivers/staging/fw-api -u FETCH_HEAD
git commit

git remote add qca-wifi-host-cmn https://git.codelinaro.org/clo/la/platform/vendor/qcom-opensource/wlan/qca-wifi-host-cmn
git fetch qca-wifi-host-cmn <TAG>
git merge -s ours --no-commit --allow-unrelated-histories FETCH_HEAD
git read-tree --prefix=drivers/staging/qca-wifi-host-cmn -u FETCH_HEAD
git commit

git remote add qcacld-3.0 https://git.codelinaro.org/clo/la/platform/vendor/qcom-opensource/wlan/qcacld-3.0
git fetch qcacld-3.0 <TAG>
git merge -s ours --no-commit --allow-unrelated-histories FETCH_HEAD
git read-tree --prefix=drivers/staging/qcacld-3.0 -u FETCH_HEAD
git commit

git remote add data https://git.codelinaro.org/clo/la/platform/vendor/qcom-opensource/data-kernel
git fetch data <TAG>
git merge -s ours --no-commit --allow-unrelated-histories FETCH_HEAD
git read-tree --prefix=techpack/data -u FETCH_HEAD
git commit

git remote add audio https://git.codelinaro.org/clo/la/platform/vendor/opensource/audio-kernel
git fetch audio <TAG>
git merge -s ours --no-commit --allow-unrelated-histories FETCH_HEAD
git read-tree --prefix=techpack/audio -u FETCH_HEAD
git commit

git fetch https://git.codelinaro.org/clo/la/kernel/msm-4.14 <TAG>
git merge FETCH_HEAD

git fetch https://android.googlesource.com/kernel/common/ upstream-f2fs-stable-linux-4.14.y
git merge FETCH_HEAD

git fetch https://android.googlesource.com/kernel/common/ android-4.14-stable
git merge FETCH_HEAD
```
