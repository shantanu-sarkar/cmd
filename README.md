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
# For LineageOS
## Dirty build
```bash
source build/envsetup.sh
breakfast guacamoleb
sudo mount --bind ~/.cache /mnt/ccache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/ccache
croot
brunch guacamoleb | tee log.txt
```
## Installclean
```bash
source build/envsetup.sh
make installclean
breakfast guacamoleb
sudo mount --bind ~/.cache /mnt/ccache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/ccache
croot
brunch guacamoleb | tee log.txt
```
# For Pixel Experience
## Dirty Build
```bash
source build/envsetup.sh
lunch aosp_guacamoleb-user
sudo mount --bind ~/.cache /mnt/ccache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/ccache
mka bacon -j$(nproc --all) | tee log.txt
```
## Installclean
```bash
source build/envsetup.sh
make installclean
lunch aosp_guacamoleb-user
sudo mount --bind ~/.cache /mnt/ccache
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/mnt/ccache
mka bacon -j$(nproc --all) | tee log.txt
```
## Sign all commits by default (Windows)
```bash
git config user.signingkey 5D570797CF704721
git config commit.gpgsign true
```
## Gerrit sample command for LOS
For pushing to gerrit
```bash
git push ssh://shantanu-sarkar@review.lineageos.org:29418/LineageOS/* HEAD:refs/for/lineage-19.1
```
For hook
```bash
gitdir=$(git rev-parse --git-dir); scp -p -P 29418 shantanu-sarkar@review.lineageos.org:hooks/commit-msg ${gitdir}/hooks/
git commit --amend --no-edit
```
## Gerrit sample command for PE
For pushing to gerrit
```bash
git push ssh://shantanu-sarkar@gerrit.pixelexperience.org:29418/* HEAD:refs/for/twelve-plus
```
For hook
```bash
gitdir=$(git rev-parse --git-dir); scp -p -P 29418 shantanu-sarkar@gerrit.pixelexperience.org:hooks/commit-msg ${gitdir}/hooks/
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
