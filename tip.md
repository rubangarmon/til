find . -iname "bin" -o -iname "obj" | xargs rm -rfv

find . -iname "bin" -o -iname "obj" -print0 | xargs -0 rm -rf

check this:

https://stackoverflow.com/questions/755382/i-want-to-delete-all-bin-and-obj-folders-to-force-all-projects-to-rebuild-everyt
