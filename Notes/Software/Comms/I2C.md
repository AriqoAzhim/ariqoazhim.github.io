# I2C

Offers an effective means of short-distance intra-board communication that is ideal for embedded systems and micro-computing applications where the primary design concerns are simplicity and low manufacturing cost.

| Advantages | Disadvantages |
| --- | --- |
| + Simple | + Can only be used effectively in short distances (up to 10m). The longer the length the slower the baud rate |

The maximum bus length of an I2C link is about **1 meter at 100 Kbaud, or 10 meters at 10 Kbaud**
. An unshielded cable typically has much less capacitance, but should only be used within an otherwise shielded enclosure.

## Implementation

### Writing from Master to Slave

1. Send a start sequence

2. Send the I2C address of the slave with the Read/Write bit low (even address)

3. Send the internal register number you want to write to

4. Send the data byte

5. [Optionally, send any further data bytes]

6. Send the stop sequence. 

### Writing from Slave to Master

### Reading from Slave from Master

### Reading from Master from Slave