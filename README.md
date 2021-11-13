## Environment
#### U-boot tag: v2020.04(Tested)
#### Enable this U-boot configuration (CONFIG_DEMO_LED):
- Add these lines into configs/am335x_evm_defconfig file
CONFIG_DEMO_LED=y
CONFIG_CMD_DEMO_LED=y
CONFIG_LED=y

#### Connect a led into the P1.14 pin

#### Clone and apply the led_uboot repo to uboot source code:
git clone git://git.denx.de/u-boot.git   
cd u-boot   
git checkout v2020.04  
cp /source/led_u-boot/cmd/* cmd/  
cp -rf /source/led_u-boot/drivers/* drivers/  
git apply /source/led_u-boot/my_custom.patch  

## Step to run demo
### 1. Compile the new u-boot with demo-led driver in it
make am335x_evm_defconfig  
make -j4
### 2. Copy MLO, u-boot.img, u-boot.bin(optional) to boot parition of SDcard
### 3. Run these command on U-boot commandline
=> demo_led show
led:P1:14
=> demo_led led:P1:14
LED 'led:P1:14': off
=> demo_led led:P1:14 on
=> demo_led led:P1:14
LED 'led:P1:14': on
=> demo_led led:P1:14 off
=> demo_led led:P1:14
LED 'led:P1:14': off
=> demo_led
demo_led - manage DEMO LEDs

Usage:
demo_led <led_label> on|off|toggle      Change LED state
demo_led <led_label>    Get LED state
demo_led show    Show LED label

 

## Crestae a led node in device-tree
demo_leds {
		pinctrl-names = "default";
		pinctrl-0 = <&demo_leds>;

		compatible = "demo-uboot-leds";

		used_led@1 {
				label = "led-1_14";
				gpios = <&gpio1 14 GPIO_ACTIVE_HIGH>;
				default-state = "off";
		};
};

## Crestae pinctrl for this node in device-tree
demo_leds: demo_leds {
		pinctrl-single,pins = <
				0x38 (PIN_OUTPUT_PULLDOWN | MUX_MODE7) /* gpmc_ad14.gpio1_14 */
		>;
};


CONFIG_DEMO_LED=y
