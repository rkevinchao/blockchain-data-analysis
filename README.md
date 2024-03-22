<!-- PROJECT LOGO -->
<a name="readme-top"></a>

<h3 align="center" style="font-size: 24px;">Ekubo Protocol (v3) Market Depth Analysis and Profit/Loss Calculation</h3>
<div>
  <p align="center" style="font-size: 22px;">
    Kevin Chao, Ph.D.
  </p>
  <p align="center">    
    Email: 
    <a href="mailto:kevinchao@gmail.com">kevinchao@gmail.com</a>
    <br /> LinkedIn:
    <a href="https://www.linkedin.com/in/kevin-chao-com/">https://www.linkedin.com/in/kevin-chao-com/</a>
    <br /> GitHub:
    <a href="https://github.com/rkevinchao">https://github.com/rkevinchao</a>
    <br /> Personal Website:
    <a href="https://kevinchao.com/">https://kevinchao.com/</a>
    <br />
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details open>
  <summary>Table of Contents</summary>
  <ol>
    <li> 
      <a href="#about-the-project">About The Project</a>.    <!-- 1. ABOUT THE PROJECT -->
    </li>
    <li><a href="#summary">Summary</a></li>                  <!-- 2. Summary -->
    <li>
      <a href="#data-methods">Data and Methods</a>           <!-- 3. Data and Methods -->
      <ul>
        <li><a href="#prerequisites">Ekubo Dataset</a></li>    <!-- 3.1 Ekubo -->
        <li><a href="#installation">Methods</a></li>           <!-- 3.2 Methods -->
      </ul>
    </li>
    <li>
      <a href="#methods">Market Depth Analysis</a>.          <!-- 4. Market Depth -->
      <ul>
        <li><a href="#prerequisites">ETH/USDC and USDC/USDT</a></li> <!--4.1 USDC/USDT-->
        <li><a href="#installation">STRK/ETH and STRK/USDC</a></li> <!--4.2 STRK/USDC-->
      </ul>
    </li>    
    <li>
    	<a href="#usage">Profit and Loss (PnL) Calculations</a></li> <!--5. PnL-->
       <ul>
        <li><a href="#prerequisites">ETH/USDC</a></li>               <!--5.1 USDC/USDT -->
        <li><a href="#installation">STRK/ETH and STRK/USDC</a></li>  <!--5.2 STRK/ETH -->
       </ul>
    <li><a href="#references">Discussion and Conclusion</a></li>
    <li><a href="#references">References</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## 1. About The Project
The goal is to analyze the market depth of `Ekubo Protocol (v3)`[[1](https://docs.ekubo.org)] and provide profit and loss (PnL) calculation. The Ekuubo Protocol is an  automated market maker (AMM) designed for `Starknet`[[2](https://www.starknet.io/en/learn/what-is-starknet)], which is a layer 2 network on Ethereum providing a decentralized platform. Its unique features include concentrated liquidity and an extensible, gas-efficient architecture.

The deliverable results of this project include 

Project Link: [https://github.com/rkevinchao/blockchain-data-analysis](https://github.com/rkevinchao/blockchain-data-analysis)

<p align="right">(<a href="#readme-top">back to top</a>)</p>


## 2. Summary
Here's a blank template to get started: To avoid retyping too much info. Do a search and replace with your text editor for the following: `github_username`, `repo_name`, `twitter_handle`, `linkedin_username`, `email_client`, `email`, `project_title`, `project_description`

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### 2.1 Market Depth of 

* [![Next][Next.js]][Next-url]
* [![React][React.js]][React-url]

### 2.2 Built With

* [![Next][Next.js]][Next-url]
* [![React][React.js]][React-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## 3. Data and Methods

This is an example of how you may give instructions on setting up your project locally.
To get a local copy up and running follow these simple example steps.

### 3.1 The Ekubo Dataset
* `BLOCK_NUMBER`: The block when the transaction occurred

This is an example of how to list things you need to use the software and how to install them.
* npm
  ```sh
  npm install npm@latest -g
  ```

	- `BLOCK_TIMESTAMP`: The timestamp of the block
	- `TX_HASH`: The unique hash of the transaction where the event occurred (a given transaction can include multiple events)
	- `TX_ID`: An identifier that indicates where in the block the transaction occurred
	- `POOL_ID`: A unique identifier of the liquidity pool
	- `TOKEN0_ADDRESS`: Starknet address of token0
	- `TOKEN1_ADDRESS`: Starknet address of token1
	- `EVENT_NAME`: The type of event (`Mint`, `Burn`, or `Swap`)
	- `FROM_ADDRESS`: The user’s address
	- `TO_ADDRESS`: The contract address
	- `TOKEN0_RAW_AMOUNT`: Raw (not decimal adjusted) number of token0 transferred
	- `TOKEN0_DECIMALS`: Number of decimal places of token0
	- `TOKEN0_REAL_AMOUNT`: `TOKEN0_RAW_AMOUNT / 10 ** TOKEN0_DECIMALS`
	- `TOKEN1_RAW_AMOUNT`: Raw (not decimal adjusted) number of token1 transferred
	- `TOKEN1_DECIMALS`: Number of decimal places of token1
	- `TOKEN1_REAL_AMOUNT`: `TOKEN1_RAW_AMOUNT / 10 ** TOKEN1_DECIMALS`
	- `FEE_TIER`: The fee rate of the pool (only Swap transactions pay fees)
	- `LIQUIDITY_AMOUNT`: The amount of liquidity added in a Mint or subtracted in a Burn between `LOWER_TICK` and `UPPER_TICK` (NB: This number is not denominated in either token. You need Uniswap-style constant product math to convert this to a token amount.)
	- `LOWER_TICK`: The lower bound of the tick range that liquidity was added to in a Mint or subtracted from in Burn
	- `UPPER_TICK`: The upper bound of the tick range that liquidity was added to in a Mint or subtracted from in Burn
	- `SWAP_TICK`: The final price tick reached after a Swap (set to 0 for all Mints and Burns)
	- `TICK_SPACING`: Defines the intervals on which users are allowed to add or subtract liquidity in the given pool (not relevant to this assignment)



### Installation

1. Get a free API Key at [https://example.com](https://example.com)
2. Clone the repo
   ```sh
   git clone https://github.com/github_username/repo_name.git
   ```
3. Install NPM packages
   ```sh
   npm install
   ```
4. Enter your API in `config.js`
   ```js
   const API_KEY = 'ENTER YOUR API';
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- USAGE EXAMPLES -->
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ROADMAP -->
## Roadmap

- [ ] Feature 1
- [ ] Feature 2
- [ ] Feature 3
    - [ ] Nested Feature

See the [open issues](https://github.com/github_username/repo_name/issues) for a full list of proposed features (and known issues).

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



<!-- LICENSE -->
## Discussion

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->
## Conclusion

Kevin Chao - [@twitter_handle](https://twitter.com/twitter_handle) - [https://github.com/rkevinchao/blockchain-data-analysis](https://github.com/rkevinchao/blockchain-data-analysis)

Project Link: [https://github.com/rkevinchao/blockchain-data-analysis](https://github.com/rkevinchao/blockchain-data-analysis)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## References
<a name="references"></a>

* [[1](https://docs.ekubo.org)] https://docs.ekubo.org
* [[2](https://www.starknet.io/en/learn/what-is-starknet)] https://www.starknet.io/en/learn/what-is-starknet
* [3]()

<p align="right">(<a href="#readme-top">back to top</a>)</p>