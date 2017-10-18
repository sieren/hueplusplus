# hueplusplus
A simple and easy to use library for Philips Hue Lights

## Features
* finds bridges with SSDP or set ip manually
* function to assign username or set manually
* all common light functions
* extended alert() function, which alerts in a specific color
* documented with doxygen

## How to use
### <a name="findingBridges"></a>Finding Bridges
If you want to find a Hue Bridge make a HueFinder object and call FindBridges()
it will return a vector containing the ip and mac address of all found Bridges.
If no Bridges were found the vector is empty, so make sure that in that case you provide an ip and mac address.
```C++
HueFinder finder;
std::vector<HueFinder::HueIdentification> bridges = finder.FindBridges();
if (bridges.empty())
{
	bridges.push_back({ "<ip address>", "<mac address>" });
}
```

### Using Bridges
If you have found the Bridge you were looking for, you can then move on with the authentication process.
To get a new username from the Bridge (for now) you simply call GetBridge(bridges[\<index\>]),
where index is your preferred Bridge from the part [Finding Bridges](#findingBridges).
```C++
Hue bridge = finder.GetBridge(bridges[0]);
```
If you on the other hand already have a username you can add your bridge like so
```C++
finder.AddUsername(bridges[0].mac, "<username>");
Hue bridge = finder.GetBridge(bridges[0]);
```

### Controlling lights
If you have your Bridge all set up, you can now control its lights.
For that create a new HueLight object and call getLight(\<id\>) on your bridge object to get a reference to a specific light, where id
is the id of the light set internally by the Hue Bridge.
```C++
HueLight light1 = bridge.getLight(1);
```
If you don't know the id of a specific light or want to get an overview over all lights that are controlled by your bridge, you can get a vector containing them by calling getAllLights() on your bridge object. If no lights are found the vector will be empty.
```C++
std::vector<std::reference_wrapper<HueLight>> lights = bridge.getAllLights();
```
If you now want to control a light, call a specific function of it.
```C++
light1.On();
light1.setBrightness(120);
light1.alertHueSaturation(25500, 255);
light1.setColorLoop(true);
light1.setColorRGB(255, 128, 0);
lights[1].Off();
lights.at(1).setColorHue(4562);
```
But keep in mind that some light types do not have all functions available. So you might call a
specific function, but nothing will happen. For that you might want to check beforehand what type
of a lamp you are controlling. For that you can call the function getColorType(), which will return
a ColorType.
```C++
ColorType type1 = light1.getColorType();
```

### Further reading
If you want to know more about all functions just look inside the doxygen description,
which for now can be found in the regarding sourcecode file or create the documentation
with the provided Doxyfile yourself.

## Build and install
If you want to build the library you can use cmake (at least version 2.8.3). First create a build folder and then execute cmake.
```bash
mkdir build
cd build
cmake ..
```
Then compile the code with make. If you are inpatient use the option -j\<number\>, where number specifies how many files are compiled at the same time. Note this number should not exceed the number of cores*2 of your machine.
```bash
make
```
```bash
make -j4
```
If you want to install the library use
```bash
make install
```
To remove it
```bash
make uninstall
```
If you additionally want to run the tests you will need to checkout the testing branch and use cmake with the option -Dhueplusplus_TESTS=ON. Testing is done with Google gtest and gmock. Note that you wont need to install gtest/gmock yourself, because cmake will automatically download them and include them during the build.
```bash
mkdir build
cd build
cmake .. -Dhueplusplus_TESTS=ON
make
```
To execute the tests use
```bash
./hueplusplus/test/HuePlusPlus_Test
```


## Copyright
Copyright (c) 2017 Jan Rogall & Moritz Wirger. See LICENSE for further details.
