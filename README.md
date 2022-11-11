<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/BadGenius22/BoH-FrontEnd">
    <img src="https://github.com/BadGenius22/BoH-FrontEnd/blob/main/src/assets/logo.svg" alt="Logo" width="502" height="295">
  </a>

  <h3 align="center">Best-README-Template</h3>

  <p align="center">
    An Ultimate Web3 Battle Card Game.
    <br />
    <a href="https://github.com/BadGenius22/BoH-FrontEnd"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/BadGenius22/BoH-FrontEnd">View Demo</a>
    ·
    <a href="https://github.com/BadGenius22/BoH-FrontEnd/issues">Report Bug</a>
    ·
    <a href="https://github.com/BadGenius22/BoH-FrontEnd/issues">Request Feature</a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

Homepage

  <a href="https://github.com/BadGenius22/BoH-FrontEnd">
    <img src="https://github.com/BadGenius22/BoH-FrontEnd/blob/main/screenshot/homepage.png" alt="Home" width="1080" height="500">
  </a>
  
 Battleground

  <a href="https://github.com/BadGenius22/BoH-FrontEnd">
    <img src="https://github.com/BadGenius22/BoH-FrontEnd/blob/main/screenshot/battleground.png" alt="Home" width="1080" height="500">
  </a>
 
  Arena

  <a href="https://github.com/BadGenius22/BoH-FrontEnd">
    <img src="https://github.com/BadGenius22/BoH-FrontEnd/blob/main/screenshot/arena.png" alt="Home" width="1080" height="500">
  </a>



This game is made for Chainlink Hackathon Fall 2022. Implements Chainlink VRFV2 & Chainlink Automation to generate rundom numbers Attack & Defend strength of the battle card. Thanks to those Chainlink technology, make the game untamperable in random numbers generated in a decentralized way, creating fair RNG for players.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

This section should list any major frameworks/libraries used to bootstrap your project. Leave any add-ons/plugins for the acknowledgements section. Here are a few examples.

* [![Next][Next.js]][Next-url]
* [![React][React.js]][React-url]
* [![Hardhat][Hardhat]][Hardhat-url]
* [![Javascript][Javascript]][Javascript-url]
* [![Typescript][Typescript]][Typescript-url]
* [![Solidity][Solidity]][Solidity-url]
* [![Chainlink][Chainlink]][Chainlink-url]
* [![OpenZeppelin][OpenZeppelin]][OpenZeppelin-url]
* [![Polygon][Polygon]][Polygon-url]


<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Blockchain




<!-- GETTING STARTED -->
## Getting Started

Go to ... to try play the game.

### Prerequisites

Before you can play the game you must:

* Install Metamask Wallet first in your browser.
 https://metamask.io/
* Add Polygon Mumbai Testnet into your Metamask Wallet. 
https://medium.com/stakingbits/how-to-connect-polygon-mumbai-testnet-to-metamask-fc3487a3871f
* Get MATIC Token(for Gas fee) for free here: https://faucet.polygon.technology/


<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- USAGE EXAMPLES -->
## Usage

Implementation of Chainlink VRFV2 & Chainlink Automation code:
```
 /*
     * @dev This is the function that the Chainlink Keeper nodes call
     * they look for `upkeepNeeded` to return True.
     */
    function checkUpkeep(
        bytes memory /*checkData*/
    )
        public
        view
        override
        returns (
            bool upkeepNeeded,
            bytes memory /*performData*/
        )
    {
        bool timePassed = ((block.timestamp - s_lastTimestamp) > i_interval);
        bool hasPlayers = players.length > 1;
        bool hasBattles = battles.length > 1;
        upkeepNeeded = (timePassed && hasPlayers && hasBattles);
        return (upkeepNeeded, "Checked!"); // can we comment this out?
    }

    function performUpkeep(
        bytes calldata /* performData */
    ) external override {
        (bool upkeepNeeded, ) = checkUpkeep("");
        if (!upkeepNeeded) {
            revert Battle__UpkeepNotNeeded(
                address(this).balance,
                players.length,
                battles.length
            );
        }
        s_battleStatus = BattleStatus.STARTED;
        uint256 requestId = i_vrfCoordinator.requestRandomWords(
            i_gasLane,
            i_subscriptionId,
            REQUEST_CONFIRMATIONS,
            i_maxGasLimit,
            NUM_WORDS
        );
        emit RequestSent(requestId);
    }

    /// @dev chainlink oracle function to generate random number; used for Battle Card Attack and Defense Strength
    function fulfillRandomWords(uint256, uint256[] memory randomWords)
        internal
        override
    {
        uint256 randomValue = (randomWords[0] % MAX_ATTACK_DEFEND_STRENGTH) + 1;
        uint256 randomValue2 = (randomWords[1] % MAX_ATTACK_DEFEND_STRENGTH) +
            1;
        s_randomValue = randomValue;
        s_randomValue2 = randomValue2;
        s_lastTimestamp = block.timestamp;
        emit RequestFulfilled(randomValue, randomValue2);
    }
```



<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>




<!-- CONTACT -->
## Contact

Dewangga Praxindo - [@dewaxindo](https://twitter.com/dewaxindo) - badgenius.crypto@gmail.com

Project Link: 
* Front-End: [https://github.com/BadGenius22/BoH-FrontEnd](https://github.com/BadGenius22/BoH-FrontEnd)
* Back-End(Smart-Contract):[https://github.com/BadGenius22/BoH-BackEnd](https://github.com/BadGenius22/BoH-BackEnd)

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

 Useful resources & tutorials I find helpful.

* [Chainlink VRFV2 Docs](https://docs.chain.link/docs/vrf/v2/introduction/)
* [Chainlink Automation Docs](https://docs.chain.link/docs/chainlink-automation/introduction/)
* [Patrick Collins](https://github.com/PatrickAlphaC)
* [Full Stack Web3 Development Course](https://www.youtube.com/watch?v=gyMwXuJrbJQ)
* [Adrian Hajdin](https://github.com/adrianhajdin)
* [Chainlink Hackaton](https://chainlinkfall2022.devpost.com/)


<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/othneildrew/Best-README-Template.svg?style=for-the-badge
[contributors-url]: https://github.com/othneildrew/Best-README-Template/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/othneildrew/Best-README-Template.svg?style=for-the-badge
[forks-url]: https://github.com/othneildrew/Best-README-Template/network/members
[stars-shield]: https://img.shields.io/github/stars/othneildrew/Best-README-Template.svg?style=for-the-badge
[stars-url]: https://github.com/othneildrew/Best-README-Template/stargazers
[issues-shield]: https://img.shields.io/github/issues/othneildrew/Best-README-Template.svg?style=for-the-badge
[issues-url]: https://github.com/othneildrew/Best-README-Template/issues
[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/othneildrew/Best-README-Template/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/dewaxindo
[product-screenshot]: images/screenshot.png
[Next.js]: https://img.shields.io/badge/next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white
[Next-url]: https://nextjs.org/
[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[React-url]: https://reactjs.org/
[Vue.js]: https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D
[Vue-url]: https://vuejs.org/
[Angular.io]: https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white
[Angular-url]: https://angular.io/
[Svelte.dev]: https://img.shields.io/badge/Svelte-4A4A55?style=for-the-badge&logo=svelte&logoColor=FF3E00
[Svelte-url]: https://svelte.dev/
[Laravel.com]: https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white
[Laravel-url]: https://laravel.com
[Bootstrap.com]: https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white
[Bootstrap-url]: https://getbootstrap.com
[JQuery.com]: https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white
[JQuery-url]: https://jquery.com 
[Solidity]: https://img.shields.io/badge/Solidity-e6e6e6?style=for-the-badge&logo=solidity&logoColor=black
[Solidity-url]: https://docs.soliditylang.org/en/v0.8.17/
[Javascript]: https://img.shields.io/badge/JavaScript-323330?style=for-the-badge&logo=javascript&logoColor=F7DF1E
[Javascript-url]: https://www.javascript.com/
[Chainlink]: https://img.shields.io/badge/chainlink-375BD2?style=for-the-badge&logo=chainlink&logoColor=white
[Chainlink-url]: https://chain.link/
[Typescript]: https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white
[Typescript-url]: https://www.typescriptlang.org/
[Twitter]: https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white
[Twitter-url]: https://twitter.com/dewaxindo
[Linktree]: https://img.shields.io/badge/linktree-39E09B?style=for-the-badge&logo=linktree&logoColor=white
[Linktree-url]: https://linktr.ee/dewaxindo
[Instagram]: https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white
[Instagram-url]: https://www.instagram.com/dewaxindo/
[Devpost]: https://img.shields.io/badge/Devpost-003E54?style=for-the-badge&logo=Devpost&logoColor=white
[Devpost-url]: https://devpost.com/BadGenius22?ref_content=user-portfolio&ref_feature=portfolio&ref_medium=global-nav
[Telegram]: https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white
[Telegram-url]: https://t.me/dewaxindo
[Polygon]: https://tinyurl.com/ys9yfcpw
[Polygon-url]: https://polygon.technology/
[Hardhat]: https://tinyurl.com/yjs68jbs
[Hardhat-url]: https://hardhat.org/
[OpenZeppelin]: https://img.shields.io/badge/OpenZeppelin-4E5EE4?logo=OpenZeppelin&logoColor=fff&style=for-the-badge
[OpenZeppelin-url]: https://www.openzeppelin.com/
