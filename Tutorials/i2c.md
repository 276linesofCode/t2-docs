# I2C

I2C is a simple-to-use communication protocol which can have more than one master, with the upper bus speed defined, and only two wires with pull-up resistors are needed to connect almost _unlimited_ number of I2C devices.

Each slave device has a unique address. Transfer from and to master device is serial and it is split into 8-bit packets. All these simple requirements make it very simple to implement I2C interface even with cheap micro-controllers that have no special I2C hardware controller. You only need 2 free I/O pins and few simple i2C routines to send and receive commands.

Let us get the accelerometer values using the I2C protocol. We would be using the [accel-mma84](https://www.seeedstudio.com/Tessel-Accelerometer-Module-p-2223.html)

![Fritzing Diagram](http://i.imgur.com/zK4U4S3.png)https://github.com/276linesofCode/Codes-Ideas/edit/master/tut-i2c.md#fork-destination-box

```js
var tessel = require('tessel'); //Import tessel

// Connect to device
var port = tessel.port.A; // Select Port A of Tessel

//This address of the Slave has been taken from https://github.com/tessel/accel-mma84/blob/master/index.js#L15
//More about registers can be found at Page 19 of https://www.nxp.com/docs/en/data-sheet/MMA8452Q.pdf
var slaveAddress = 0x1D; // Specefic for accelerometer module
var i2c = new port.I2C(slaveAddress); // Initialize I2C communication

// Details of I2C transfer
var numBytesToRead = 1; // Read back this number of bytes

i2c.read(numBytesToRead, function (error, dataReceived) {

  // Print data received (buffer of hex values)
  console.log('Buffer returned by I2C slave device ('+slaveAddress.toString(16)+'):', dataReceived);
  
});

// Read/Receive data over I2C using i2c.transfer
// 0x0D is the WHO_AM_I Register which sends back an acknoledgement to the master for starting the communication
i2c.transfer(new Buffer([0x0D]), numBytesToRead, function (error, dataReceived) {

    // Print data received (buffer of hex values)
    // The returned buffer from the I2C slave device should be [0x2A]
  console.log('Buffer returned by I2C slave device ('+slaveAddress.toString(16)+'):', dataReceived);
  
});


// Now, try to print the accelerometer data using i2c.transfer
// The register address for OUT_X_MSB is 0x01. This can be found at Page 19 of https://www.nxp.com/docs/en/data-sheet/MMA8452Q.pdf
// 6 Bytes are used for pairwise MSB and LSB of the x,y and z axis
i2c.transfer(new Buffer([0x01]), 6, function (error, dataReceived) {

    if (error) throw error;
    //Create a blank array for the output
    var out=[];
    //iterating three times the x, y, z values
    for (var i=0;i<3;i++){
      var gCount=(dataReceived[i*2] << 8) | dataReceived[(i*2)+1]; //Converting the 8 bit data into a 12 bit
      gCount=gCount >> 4;
      if (dataReceived[i*2] > 0x7F) {
          gCount = -(1 + 0xFFF - gCount); // Transform into negative 2's complement
      }
        out[i] = gCount / ((1<<12)/(2*2));
    }
    console.log('The x, y, z values are :',out); //Log the Array containing the x,y,z values
    
});
```

[Datasheet for accelerometer module](http://www.nxp.com/docs/en/data-sheet/MMA8452Q.pdf)

[More Informatiom on I2C Communication](https://learn.sparkfun.com/tutorials/i2c)