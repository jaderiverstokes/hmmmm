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

[sample code](https://github.com/jaderiverstokes/bitmix/blob/main/src/index.js#L261)
