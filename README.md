# hmmmm
honestly mindless market maker memorandum

## literature review

### the genesis
[uniswap](https://hackmd.io/@HaydenAdams/HJ9jLsfTz#Creating-Exchanges)

<img width="384" alt="image" src="https://user-images.githubusercontent.com/9206704/159773793-ab580fa5-4326-4c01-b7d8-d84bf5b2252f.png">



literature review

### design considerations:

- ease-of-use
- gas efficiency
- censorship resistance
- zero rent extraction
- particularily

### legacy markets:

- order book
- match buyer and seller

### current markets:

- manage liquidity reserves
- execute trades against reserves
- prices set automagically

### constant product:
- ![x*y=k](https://latex.codecogs.com/svg.image?x*y=k)
- keep 'relative equilibrium'
- network of liquidity providers
- exchange for transaction fees
- [formal spec](https://github.com/runtimeverification/verified-smart-contracts/blob/uniswap/uniswap/x-y-k.pdf)

### factory + registry

- deploys 1 exchange for each erc20
- exchange holds ETH & erc20 reserves
- exchanges linked through registry
- erc20->erc20 using ETH intermediary


minimalist design
20% overhead compared to erc20 token t
ransfer 🤯 

```python 
createExchange(token: address) 
getExchange(token: address) 
getToken(exchange: address) 
```
![eth{\textunderscore}pool * token{\textunderscore}pool = invariant](https://latex.codecogs.com/svg.image?eth{\textunderscore}pool&space;*&space;token{\textunderscore}pool&space;=&space;invariant)

- held constant during trades
- changes when liquidity add/removed

```python
def ethToTokenSwap():
    fee = msg.value / 500
    invariant = eth_pool * token_pool
    new_eth_pool = eth_pool + msg.value
    new_token_pool = invariant / (new_eth_pool - fee)
    tokens_out = token_pool - new_token_pool
    eth_pool = new_eth_pool
    token_pool = new_token_pool
    token.transfer(msg.sender, tokens_out)
```

1. ETH sent into eth_pool
1. token_pool decreases proportionally
1. reserve ratio shifts
1. reverse trades are incentivized


## eth->erc20 example

ETH -> OMG

1. liquidity

- 10 ETH & 500 OMG deposited by LPs
- invariant = 10 * 500 = 5000

1. trade

```
Buyer -> 1 ETH
Fee: 1 ETH/ 500 = 0.0025 ETH
ETH_pool <- 10 + 1 - 0.0025 = 10.9975 ETH
OMG_pool <- 5000/10.9975 = 454.65 ETH
Buyer <- 500 - 454.65 = 45.35 OMG
```

1. fee

```
ETH_pool = 10.9975 + 0.0025 = 11 ETH
OMG_pool = 454.65 OMG
new_invariant <- 11 * 454.65 = 5001.15
```

1. price

first buyer 45.35 OMG/ETH
next buyer has a less favorable rate
next seller has a more favorable rate
large trades cause more price slippage
arbitrage creates price discovery

## erc20->erc20 example
- tokenToEth * ethToToken = tokenToToken
<img width="503" alt="image" src="https://user-images.githubusercontent.com/9206704/159801732-34277db4-e92d-414a-bd04-af1554d42053.png">


- ![amountMinted = totalAmount * \frac{ethDeposited}{ethPool}](https://latex.codecogs.com/svg.image?amountMinted&space;=&space;totalAmount&space;*&space;\frac{ethDeposited}{ethPool})

[sample code](https://github.com/jaderiverstokes/bitmix/blob/main/src/index.js#L261)
