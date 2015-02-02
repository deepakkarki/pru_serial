PRU Serial
==========

The aim of this project is to create a multi channel messaging library for the PRU. Having a serial interface makes working with the arduino really simple, pru_serial aims to do the same for the PRU. This is quite useful while debugging PRU firmware and generally to stream data to the Linux userspace. 

**Status** : Under Implementation.

##API (proposed)

*The API's can change, suggestions welcome!*

####From the PRU side :

```c
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

####From the Linux side : 

In python
```python
      import pru
      pru.send("data", channel) #direct write to channel
      #internally calls low level fn : pru.downcall(down_call_no, *args) => causes downcall to PRU
```

In Node
```js
      var pru = require("pru");
      pru.read(channel, function (err, data) { /*callback*/ });
```

##Implementation 

This will be a nice working example a messaging system between asymmetric multi-processors. On the Linux side there will the kernel driver that interacts with the PRU. It will expose a sysfs entry to the userspace for read, write etc.

Internally there will be a set of shared memory buffers. Data will be written into these buffers in a cyclic manner. One of the buffer will be reserved as the **control data buffer** which is used by the kernel **and** the PRU to share control data (over flow, read head, write head, etc).

The driver will expose number of sysfs files, each corresponding to a channel number.
