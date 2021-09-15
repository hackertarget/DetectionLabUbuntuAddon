<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/othneildrew/Best-README-Template">
    <img src="https://assets.ubuntu.com/v1/ad9a02ac-ubuntu-orange.gif" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">DetectionLab Ubuntu Addon Host</h3>

  <p align="center">
    An unofficial addon Ubuntu server for the DetectionLab project. 
    <br />
    <a href="https://detectionlab.network/"><strong>DetectionLab Project »</strong></a>
    <br />
  </p>
</p>


<!-- ABOUT THE PROJECT -->
## About The Project

[DetectionLab](https://twitter.com/DetectionLab) is a great Cyber Security Testing Lab by [Chris Long](https://github.com/clong)

Comprised of 3 Windows and an Ubuntu (SIEM) logging host the Lab is a quick way to test the preinstalled security projects. DetectionLab uses Vagrant to build the custom virtual machines. Expanding the lab for custom scenarios is very beneficial.

Here's why:
* Save time
* Play with Open Source Security Tools and Systems
* Learn

Use these configuration files and the Vagrantfile as a template for spinning up additional Ubuntu or other Linux based hosts. The host will have osquery and ossec both pre-installed and reporting to the **Logger** host in the Lab network.

<!-- GETTING STARTED -->
## Getting Started

### Prerequisites

The Vagrant configuration file is specifically for a Virtualbox system and has been tested running on an Ubuntu 20.04 host. It will need modification to run on other hypervisors and operating systems.


### Installation

1. Clone the repo
   ```sh
   git clone https://github.com/hackertarget/DetectionLabUbuntuAddon.git
   ```
2. Build the Host with Vagrant
   ```sh
   test@vbox:~/DetectionLab/UbuntuAddon/ $ vagrant up
   ```
3. Check in the Fleet console that osquery for ubuntu200 has been enrolled


## Further Usage

Use this configuration as a template for other hosts and vagrant builds you would like to add to your DetectionLab network.


<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE` for more information.



<!-- CONTACT -->
## Contact

Peter - [https://hackertarget.com](https://hackertarget.com/)


<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
* [Centurion](https://twitter.com/centurion)
* [DetectionLab Project](https://detectionlab.network)
* [DetectionLab Github](https://github.com/clong/DetectionLab)


