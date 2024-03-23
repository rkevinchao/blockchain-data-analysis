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
    <br /> Website:
    	<a href="https://kevinchao.com">https://kevinchao.com</a>
    <br />
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details open>
  <summary>Table of Contents</summary>
  <ol>
    <li> 
      <a href="#about-the-project">About The Project</a>      <!-- 1. ABOUT THE PROJECT -->
    </li>
    <li><a href="#summary">Summary</a></li>                   <!-- 2. Summary -->
    <li>
      <a href="#ekubo-dataset">Ekubo Dataset</a>              <!-- 3. Ekubo DataSet -->
      <ul>
        <li><a href="#data-structure">Data Structure</a></li>    <!-- 3.1 Data Structure-->
        <li><a href="#data-overview">Data Overview</a></li>      <!-- 3.2 Data Overview-->
      </ul>
    </li>
    <li>
      <a href="#methods">Methods</a>                                              <!-- 4. Methods -->
      <ul>
        <li><a href="#methods-1">Methods of Computing Aggregation Dataset</a></li>    <!-- 4.1 Methods-1 -->
        <li><a href="#methods-2">Methods-2</a></li>                                   <!-- 4.2 Methods-2 -->
      </ul>
    </li>
    <li>
      <a href="#market-depth-analysis">Market Depth Analysis</a>  <!-- 4. Market Depth -->
      <ul>
        <li><a href="#eth-usdc">ETH/USDC</a></li>    <!--4.1 ETH/USDC-->
        <li><a href="#usdc-usdt"> USDC/USDT</a></li> <!--4.2 USDC/USDT-->
        <li><a href="#strk-eth">STRK/ETH</a></li>    <!--4.3 STRK/USDC-->
        <li><a href="#strk-usdc">STRK/USDC</a></li>  <!--4.4 STRK/USDC-->
      </ul>
    </li>    
    <li>
    	<a href="#pnl">Profit and Loss (PnL) Calculations</a></li>  <!--5. PnL-->
       <ul>
        <li><a href="#pnl-usdc-usdt">ETH/USDC</a></li>                 <!--5.1 USDC/USDT -->
        <li><a href="#pnl-strk-eth">STRK/ETH and STRK/USDC</a></li>    <!--5.2 STRK/ETH -->
       </ul>
    <li><a href="#discussion-conclusion">Discussion and Conclusion</a></li>   <!--6. Discussion and Conclusion-->
    <li><a href="#references">References</a></li>                             <!--7. References-->
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
<a name="about-the-project"></a>
## 1. About The Project
The goal is to analyze the market depth of `Ekubo Protocol (v3)`[[1](https://docs.ekubo.org)] and provide profit and loss (PnL) calculation. The Ekuubo Protocol is an  automated market maker (AMM) designed for `Starknet`[[2](https://www.starknet.io/en/learn/what-is-starknet)], which is a layer 2 network on Ethereum providing a decentralized platform. Its unique features include concentrated liquidity and an extensible, gas-efficient architecture.

The targeted tasks consist of two parts: (1) Providing market depth for the Ekubo Protocol for STRK/ETH, STRK/USDC, ETH/USDC, and USDC/USDT pools. (2) Calculating the Profit and Loss (PnL) for a set of hypothetical positions held by the most profitable liquidity providers.

The deliverable results include a comprehensive report that details the methodology, findings, and recommendations. This report would be supported by data visualizations and code within a Jupyter Notebook to enhance the analysis.

Project Link: [https://github.com/rkevinchao/blockchain-data-analysis](https://github.com/rkevinchao/blockchain-data-analysis)
<p align="right">(<a href="#readme-top">back to top</a>)</p>

<a name="summary"></a>
## 2. Summary
Here is the summary of this report. Will update in the last stage.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- GETTING STARTED -->
<a name="ekubo-dataset"></a>
<a name="data-structure"></a>

## 3. Ekubo Dataset

### 3.1 Data Structure
The entire Ekubo Protocol dataset is stored in a Parquet file (which is not an open dataset), containing various data such as transaction details, liquidity events, and price movements. Below is a detailed explanation for each column.

* `BLOCK_NUMBER`: The block when the transaction occurred
* `BLOCK_TIMESTAMP`: The timestamp of the block
* `TX_HASH`: The unique hash of the transaction where the event occurred (a given transaction can include multiple events)
* `TX_ID`: An identifier that indicates where in the block the transaction occurred
* `POOL_ID`: A unique identifier of the liquidity pool
* `TOKEN0_ADDRESS`: Starknet address of token0
* `TOKEN1_ADDRESS`: Starknet address of token1
* `EVENT_NAME`: The type of event (`Mint`, `Burn`, or `Swap`)
* `FROM_ADDRESS`: The user’s address
* `TO_ADDRESS`: The contract address
* `TOKEN0_RAW_AMOUNT`: Raw (not decimal adjusted) number of token0 transferred
* `TOKEN0_DECIMALS`: Number of decimal places of token0
* `TOKEN0_REAL_AMOUNT`: `TOKEN0_RAW_AMOUNT / 10 ** TOKEN0_DECIMALS`
* `TOKEN1_RAW_AMOUNT`: Raw (not decimal adjusted) number of token1 transferred
* `TOKEN1_DECIMALS`: Number of decimal places of token1
* `TOKEN1_REAL_AMOUNT`: `TOKEN1_RAW_AMOUNT / 10 ** TOKEN1_DECIMALS`
* `FEE_TIER`: The fee rate of the pool (only Swap transactions pay fees)
* `LIQUIDITY_AMOUNT`: The amount of liquidity added in a Mint or subtracted in a Burn between `LOWER_TICK` and `UPPER_TICK` (NB: This number is not denominated in either token. You need Uniswap-style constant product math to convert this to a token amount)
* `LOWER_TICK`: The lower bound of the tick range that liquidity was added to in a Mint or subtracted from in Burn
* `UPPER_TICK`: The upper bound of the tick range that liquidity was added to in a Mint or subtracted from in Burn
* `SWAP_TICK`: The final price tick reached after a Swap (set to 0 for all Mints and Burns)
* `TICK_SPACING`: Defines the intervals on which users are allowed to add or subtract liquidity in the given pool (not relevant to this assignment)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<a name="data-overview"></a>
### 3.2 Data Overview

* <a href="https://github.com/rkevinchao/blockchain-data-analysis/blob/main/01a_dataset_overview.ipynb">01a\_dataset\_overview.ipynb</a>: This Jypyter Notebook includes codes to generate results shown in this session. 

* An overview of the dataframe structure:

`df.info()`

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3024741 entries, 0 to 3024740
Data columns (total 22 columns):
 #   Column              Dtype         
---  ------              -----         
 0   BLOCK_NUMBER        object        
 1   BLOCK_TIMESTAMP     datetime64[ns]
 2   TX_HASH             object        
 3   TX_ID               object        
 4   POOL_ID             object        
 5   TOKEN0_ADDRESS      object        
 6   TOKEN1_ADDRESS      object        
 7   EVENT_NAME          object        
 8   FROM_ADDRESS        object        
 9   TO_ADDRESS          object        
 10  TOKEN0_RAW_AMOUNT   object        
 11  TOKEN0_DECIMALS     object        
 12  TOKEN0_REAL_AMOUNT  object        
 13  TOKEN1_RAW_AMOUNT   object        
 14  TOKEN1_DECIMALS     object        
 15  TOKEN1_REAL_AMOUNT  object        
 16  FEE_TIER            float32       
 17  LIQUIDITY_AMOUNT    object        
 18  LOWER_TICK          object        
 19  UPPER_TICK          object        
 20  SWAP_TICK           object        
 21  TICK_SPACING        object        
dtypes: datetime64[ns](1), float32(1), object(20)
memory usage: 496.2+ MB
```

* A quick view of the values in one randomly selected row:

`df.iloc[100]`

```
BLOCK_NUMBER                                                     317634
BLOCK_TIMESTAMP                                     2023-10-13 03:18:38
TX_HASH               0x008dcce8b4d0814d1655d294aa9a6ba898ece028e317...
TX_ID                                              5461067_0_317634_132
POOL_ID               0x00000005dd3d2f4429af886cd1a3b08289dbcea99a29...
TOKEN0_ADDRESS        0x053c91253bc9682c04929ca02ed00b3e423f6710d2ee...
TOKEN1_ADDRESS        0x068f5c6a61780768455de69077e07e89787839bf8166...
EVENT_NAME                                                         Mint
FROM_ADDRESS          0x057ddd233caca940b84ea0c51c3905eb9ca3f4ea31bc...
TO_ADDRESS            0x00000005dd3d2f4429af886cd1a3b08289dbcea99a29...
TOKEN0_RAW_AMOUNT                                            -279110619
TOKEN0_DECIMALS                                                       6
TOKEN0_REAL_AMOUNT                  -279.110618999999985589965945109725
TOKEN1_RAW_AMOUNT                                            -279094071
TOKEN1_DECIMALS                                                       6
TOKEN1_REAL_AMOUNT                  -279.094070999999985360773280262947
FEE_TIER                                                         0.0005
LIQUIDITY_AMOUNT                                            28050032209
LOWER_TICK                                                       -20000
UPPER_TICK                                                        20000
SWAP_TICK                                                             0
TICK_SPACING                                                       1000
Name: 100, dtype: object
```

* SS


* EE
* 
<img src="images/fig_01a_01_bar_plot.png" alt="Bar Plot">

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<a name="methods"></a>
## 4. Methods

<p align="right">(<a href="#readme-top">back to top</a>)</p>



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

<!-- USAGE EXAMPLES -->
<a name="market-depth-analysis"></a>
## 5. Market Depth Analysis

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<a name="eth-usdc"></a>
### 5.1 ETH/USDC

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<a name="usdc-usdt"></a>
### 5.2 USDC/USDT

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<a name="strk-eth"></a>
### 5.3 STRK/ETH

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<a name="strk-usdc"></a>
### 5.4 STRK/USDC

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ROADMAP -->
<a name="pnl"></a>
## 6. Profit and Loss (PnL) Calculations

- [ ] Feature 1
- [ ] Feature 2
- [ ] Feature 3
    - [ ] Nested Feature

See the [open issues](https://github.com/github_username/repo_name/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<a name="pnl-usdc-usdt"></a>
### 6.1 STRK/USDC

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<a name="pnl-strk-eth"></a>
### 6.2 STRK/USDC

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- CONTRIBUTING -->
<a name="discussion-conclusion"></a>
## 7. Discussion and Conclusion

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- ACKNOWLEDGMENTS -->
## 8. References
<a name="references"></a>

* [[1](https://docs.ekubo.org)] https://docs.ekubo.org
* [[2](https://www.starknet.io/en/learn/what-is-starknet)] https://www.starknet.io/en/learn/what-is-starknet
* [3]()

<p align="right">(<a href="#readme-top">back to top</a>)</p>