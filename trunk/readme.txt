## 1. deploy OpenStack from scrach.
Only checkout and run it! 
1. Setup a fresh Ubuntu Precise(12.04) OS. 

2. Clone onestack:
svn checkout http://onestack.googlecode.com/svn/trunk/ onestack-read-only

3. run it.
cd onestack-read-only/ && ./oneStack.sh

## 2. delete OpenStack
./delStack.sh

## 3. delete all
./delAll.sh

## 4. reset OpenStack
./resetStack.sh clear
./resetStack.sh

## 5. add OpenStack compute node
./addComputeNode.sh

## 6. add OpenStack client manage node
./addClient.sh

## 7. otherwise, contact me at Hily.Hoo@gmail.com, thanks.

