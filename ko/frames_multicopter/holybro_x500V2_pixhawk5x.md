# Holybro X500 V2 (Pixhawk 5X 조립)

이 페이지에서는 [Holybro X500 V2 ARF 키트](http://shop.holybro.com/x500-v2-kit_p1288.html)를 조립 방법과 *QGroundControl*의 PX4 설정 방법을 설명합니다.

ARF("거의 비행 준비 완료") 키트는 하드웨어 설정에 많은 시간을 할애하지 않고, 드론 개발에 입문자들이 간편하고 간단한 조립할 수 있습니다. 여기에는 프레임, 모터, ESC, 프로펠러 및 배전반이 포함됩니다.

키트 외에도 비행 콘트롤러, 무선 송신기, GPS 및 RC 콘트롤러가 필요합니다. ARF 키트는 PX4와 호환되는 대부분의 비행 콘트롤러를 지원합니다.

## 주요 정보

- **키트:** [Holybro X500 V2 ARF 키트](http://shop.holybro.com/x500-v2-kit_p1288.html)
- **비행 콘트롤러:** [Pixhawk 5X](../flight_controller/pixhawk5x.md)
- **조립 시간(약):** 55분(프레임 25분, 자동조종장치 설치/설성은 30분)

![Full X500 V2 Kit](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/x500-kit.png)

## 부품 명세서

Holybro [X500 V2 키트](http://shop.holybro.com/x500-v2-kit_p1288.html)에는 필수 부품들이 포함되어 있습니다.

* X500V2 프레임 키트
  * 바디 - 풀 카본 파이버 탑 & 바닥판(144 x 144mm, 두께 2mm)
  * 암 - 고강도 & 초경량 16mm 탄소 섬유 튜브
  * 랜딩 기어 - 16mm & 10mm 직경의 탄소 섬유 튜브
  * 플랫폼 보드 - GPS & 인기있는 보조 컴퓨터
  * 이중 10mm Ø 로드 x 250mm 롱 레일 마운팅 시스템
  * 2개의 배터리 스트랩이 있는 배터리 마운트
  * 설치용 수공구
* [Holybro 모터 - 2216 KV880 x6](https://shop.holybro.com/motor2216-880kv-1pc_p1154.html)
* [Holybro BLHeli S ESC 20A x4](https://shop.holybro.com/blheli-s-esc-20a_p1143.html)
* [프로펠러 - 1045x4](https://shop.holybro.com/propeller10452pair_p1155.html)
* 배전반 – 배터리 및 배터리용 XT60 플러그 ESC &용 XT30 플러그 주변기기
* 카메라 마운트(선택 사항 및 3D 파일은 [여기](http://www.holybro.com/3D_Print/Holybro_X500_V2_3D%20Print.rar)에서 다운로드할 수 있음)

이 빌드의 다른 부품(**ARF 키트에 포함되지 않음**):
* [Pixhawk 5X 자동조종장치](../flight_controller/pixhawk5x.md)
* [M8N GPS](http://shop.holybro.com/holybro-m8n-gps_p1094.html)
* [전원 모듈 - PM02D](http://shop.holybro.com/pm02d-power-module_p1285.html)
* [433/915 MHz 무선 텔레메트리](http://shop.holybro.com/sik-telemetry-radio-v3_p1103.html)

Additionally you will need a battery (Holybro recommends a 4S 5000mAh) and receiver ([compatible radio system](../getting_started/rc_transmitter_receiver.md)) if you want to control the drone manually.

## 키트 하드웨어

프레임 및 자동조종장치 설치를 위한 하드웨어 목록입니다.

| 항목                           | 설명                                                | 수량 |
| ---------------------------- | ------------------------------------------------- | -- |
| Bottom plate                 | Carbon fiber (2mm thick)                          | 1  |
| Top plate                    | Carbon fiber (1.5mm thick)                        | 1  |
| Arm                          | Carbon fiber tube (Assembled with motors mounted) | 4  |
| Landing gear - Vertical pole | Carbon fiber tube + engineering plastic           | 2  |
| Landing gear - Cross bar     | Carbon fiber tube + engineering plastic + foam    | 2  |
| Mounting Rail                | Diameter: 10mm length: 250mm                      | 2  |
| Battery mounting board       | Thickness: 2mm                                    | 1  |
| Battery pad                  | 3mm Silicone sheet black                          | 1  |
| Platform board               | Thickness: 2mm                                    | 1  |
| Hanger & rubber ring gasket  | Inner hole diameter: 10mm black                   | 8  |

 ![X500V2 ARF Kit Full Package Contents](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/x500_v2_whats_inside.png)

   _Figure 1_: X500 V2 ARF Kit what's inside

### Electronics

| Item Description                                        | Quantity |
| ------------------------------------------------------- | -------- |
| Pixhawk5x & Assorted Cables                             | 1        |
| M8N GPS Module                                          | 1        |
| Power Module PM02D (with pre-soldered ESC power cables) | 1        |
| Motors 2216 KV880（V2 Update)                            | 4        |
| Holybro BLHeli S ESC 20A x4                             | 1        |
| 433MHz Telemetry Radio / 915MHz Telemetry Radio         | 1        |

### Tools needed

Tools are included to do the assembly, however you may need:

- Wire cutters
- Precision tweezers


## Assembly

Estimate time to assemble is 55 min (25 minutes for frame, 30 minutes for autopilot installation/configuration)

1. Start by assembling the payload & battery holder. Push the rubbers into grippers (Do not use sharp items to push them in!). Next, pass the holders through the holder bars with the battery holder bases as Figure 3.

   ![Landing Figure 1: Components](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/payload_holder_required_stuff.png)

   _Figure 2_: Payload holder components

   ![Landing Figure 2: Assembled](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/payload_holder_assembled.png)

   _Figure 3_: Payload holder assembled

1. The next is to go for attaching the bottom plate to the payload holder.

   You will need the parts as shown in Figure 4. Then mount the base for power distribution board using nylon nuts as Figure 5. Finally using 8 hex screws you can join the bottom plate to the payload holder (Figure 7)

   ![Materials to attach bottom plate](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/topplate_holder_stuff.png)

   _Figure 4_: Needed Materials

   ![PDB mountbase](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/powerboard-mountbase.png)

   _Figure 5_: PDB mount base

   ![PDB attachment](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/pdb_bottom_plate.png)

   _Figure 6_: Mounted pdb with nylon nuts

   ![Bottom plate Final](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/bottom_plate_holder_final.png)

   _Figure 7_: Mounted Plate on payload holder

1. Let's gather the stuff needed for mounting landing gear as Figure 8. Use the hex screws to join landing gears to the bottom plate. You also need to open three hex screws on each of the leg stands so you can push them into carbon fiber pipes. Do not forget to tighten them back again.

   ![Attach Landing Gear Stuff](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/landing_gear_materials.png)

   _Figure 8_: Required parts for landing gear attachment

   ![Lanfing great to bottom plate](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/attached_landing_gear.png)

   _Figure 9_: Landing gear attachment to the body

1. We will gather all the arms now to mount the top plate. Please pay attention that the motor numbers on arms are a match with the ones mentioned on the top plate. Fortunately, motors are mounted and ESCs have been connected in advance. Start by passing through all the screws as you have the arms fixed in their own places (They have a guide as shown in Figure 11 to ensure they are in place) and tighten all nylon nuts a bit. Then you can connect XT30 power connectors to the power board. Please keep in mind that the signal wires have to be passed through the top plate such that we can connect them later to Pixhawk.

   <img src="../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/needed_stuff_top_plate.png" width="700" title="Arms and top plate materials" />

   _Figure 10_: Connecting arms needed materials.

   <img src="../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/guide_for_arm_mount.png" width="700" title="Guide for the arms mount" />

   _Figure 11_: Guide for the arms mount

1. Tighten all 16 screws and nuts by using both hex wrench and nut driver.

   ![Top plae mounted](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/finalized_top_plate.png)

   _Figure 12_: Mounted top plate


1. Next you can mount your pixhawk on the top plate by using the stickers. It is recommended to have the direction of your Pixhawk's arrow the same as the one mentioned on the top plate.

   ![Flight controller mounting stickers](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/pixhawk5x_stickertapes.png)

   _Figure 13_: Sticker tapes on Pixhawk

1. If you want to mount the GPS on the companion computer plate, you can now secure the GPS mount onto it using 4 screws and nuts.

   <img src="../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/gps_mount_plate.png" width="400" title="Secure GPS mount onto companion plate" />

   _Figure 14_: Secure GPS mount onto companion plate


1. Use the tape and stick the GPS to the top of the GPS mast and mount the GPS mast. Make sure the arrow on the gps is pointing forward (Figure 15).

   <img src="../../assets/airframes/multicopter/x500_holybro_pixhawk4/gps2.jpg" width="400" title="Figure 16: GPS and mast" />

   _Figure 15_: GPS and mast


1. Finally, you can connect the Pixhawk interfaces such as telemetry radio to 'TELEM1' and motors signal cables accordingly.

Please refer to [Pixhawk 5X Quick Start](../assembly/quick_start_pixhawk5x.md) for more information.

That's it. The fully assembled kit is shown below (Depth camera not included in the kit):

![Assembled Kit](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/finalized_x500v2_kit.png)


<a id="configure"></a>

## Install/Configure PX4

:::tip
Full instructions for installing and configuring PX4 can be found in [Basic Configuration](../config/README.md).
:::

*QGroundControl* is used to install the PX4 autopilot and configure/tune it for the X500 frame. [Download and install](http://qgroundcontrol.com/downloads/) *QGroundControl* for your platform.

First update the firmware and airframe:
* [Firmware](../config/firmware.md)
* [Airframe](../config/airframe.md) - You will need to select the *Holybro X500 V2* airframe (**Quadrotor x > Holybro X500 V2**) ![QGroundControl - Select HolyBro S500 airframe](../../assets/airframes/multicopter/x500_v2_holybro_pixhawk5x/x500v2_airframe_qgc.png)

Then perform the mandatory setup/calibration:
* [Sensor Orientation](../config/flight_controller_orientation.md)
* [Compass](../config/compass.md)
* [Accelerometer](../config/accelerometer.md)
* [Level Horizon Calibration](../config/level_horizon_calibration.md)
* [Radio Setup](../config/radio.md)
* [Flight Modes](../config/flight_mode.md)

Ideally you should also do:
* [ESC Calibration](../advanced_config/esc_calibration.md)
* [Battery](../config/battery.md)
* [Safety](../config/safety.md)


## Tuning

Airframe selection sets *default* autopilot parameters for the frame. These are good enough to fly with, but it is a good idea to tune the parameters for a specific frame build. For instructions on how, see: [Multicopter Basic PID Tuning](../config_mc/pid_tuning_guide_multicopter_basic.md).

## Acknowledgements

This build log was provided by PX4 Team.
