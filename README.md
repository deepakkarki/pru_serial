PRU Serial
==========

The aim of this project is to create an arduino like serial port library for the PRU. The serial channels are meant to communicate with the linux host, not an actual hardware device. This is quite useful while debugging PRU firmware and generally to stream data to the Linux userspace. 

**Status** : Under Implementation.

##API (proposed)

**The API's can change, suggestions welcome!**

From the PRU side :
```
      //create a channel ch
      int serial_create_channel(int ch);
      
      //write to a channel
      int serial_write(int ch, char* data, int len);
      
      //read from a channel 
      int serial_read(int ch, char* buff, int len);
      
      //make the read blocking or non blocking. Returns previous value.
      int set_blocking_read(int ch, int true_or_false);
      
      //make the write blocking or non blocking. Returns previous value.
      int set_blocking_write(int ch, int true_or_false);

```

From the Linux side : 

```
      The channels will be represented as files by the kernel driver. So normal IO operations will work :)
```

##Implementation 

This will be a nice working example a messaging system between asymmetric multi-processors. On the Linux side there will the kernel driver that interacts with the PRU. It will expose a sysfs entry to the userspace for read, write etc.

Internally there will be a set of shared memory buffers. Data will be written into these buffers in a cyclic manner. One of the buffer will be reserved as the **control data buffer** which is used by the kernel **and** the PRU to share control data (over flow, read head, write head, etc).

