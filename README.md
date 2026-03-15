# xrp-ledger-api
XRP Ledger API — Build on the XRPL DEX in Minutes

  The XRP Ledger has a native DEX, a built-in AMM, protocol-level NFTs, and 3-5 second finality. The https://xrpl.to/docs gives you access
  to all of it through 232 REST endpoints and WebSocket streams — no node required, no smart contracts, free tier included.

  Quick Start

  No API key needed. No SDK. Just fetch:

  const res = await fetch('https://api.xrpl.to/v1/tokens?limit=10');
  const { tokens } = await res.json();
  tokens.forEach(t => console.log(t.name, '$' + t.price, t.volume_24h));

  That's a live token tracker in three lines.

  What You Can Build

  ┌───────────────────────┬─────────────────────────────────────────┬──────────────┐
  │        Project        │             Endpoints Used              │  Difficulty  │
  ├───────────────────────┼─────────────────────────────────────────┼──────────────┤
  │ Token price tracker   │ /v1/tokens, /v1/token/{id}              │ Beginner     │
  ├───────────────────────┼─────────────────────────────────────────┼──────────────┤
  │ Portfolio dashboard   │ /v1/account/{address}/balances          │ Beginner     │
  ├───────────────────────┼─────────────────────────────────────────┼──────────────┤
  │ DEX trading interface │ /v1/orderbook, /v1/amm/pools            │ Intermediate │
  ├───────────────────────┼─────────────────────────────────────────┼──────────────┤
  │ Price alert bot       │ WebSocket tokens channel                │ Intermediate │
  ├───────────────────────┼─────────────────────────────────────────┼──────────────┤
  │ NFT analytics tool    │ /v1/nft/collections, /v1/nft/floors     │ Intermediate │
  ├───────────────────────┼─────────────────────────────────────────┼──────────────┤
  │ Arbitrage scanner     │ /v1/orderbook, /v1/amm/pools, WebSocket │ Advanced     │
  ├───────────────────────┼─────────────────────────────────────────┼──────────────┤
  │ Trading bot           │ /v1/orderbook, /v1/submit, WebSocket    │ Advanced     │
  └───────────────────────┴─────────────────────────────────────────┴──────────────┘

  API Overview

  Token Data

  Real-time prices, OHLC charts, holder analysis, market metrics, RSI, sparklines, trending tokens, and new listings.

  // Get top tokens with full market data
  const res = await fetch('https://api.xrpl.to/v1/tokens?limit=20');
  const { tokens } = await res.json();

  tokens.forEach(token => {
    console.log(
      token.name.padEnd(15),
      ('$' + token.price.toFixed(6)).padEnd(14),
      ('Vol: $' + (token.volume_24h || 0).toLocaleString()).padEnd(20),
      'Holders: ' + (token.holders || 0).toLocaleString()
    );
  });

  // Get OHLC chart data for a specific token
  const res = await fetch('https://api.xrpl.to/v1/token/{id}/ohlc?interval=1h');
  const candles = await res.json();
  candles.forEach(c => console.log(c.time, c.open, c.high, c.low, c.close));

  DEX Trading

  Full orderbook depth, AMM pool data, swap quotes, and trade history.

  // Get orderbook for any trading pair
  const res = await fetch('https://api.xrpl.to/v1/orderbook/XRP/USD');
  const { bids, asks } = await res.json();

  console.log('Best bid:', bids[0].price, '| Best ask:', asks[0].price);
  console.log('Spread:', (asks[0].price - bids[0].price).toFixed(6));

  // Browse AMM liquidity pools
  const res = await fetch('https://api.xrpl.to/v1/amm/pools');
  const { pools } = await res.json();

  pools.forEach(p => {
    console.log(p.pair.padEnd(20), 'TVL: $' + p.tvl, 'Volume 24h: $' + p.volume_24h);
  });

  Account & Wallet

  Balances with USD values, trustlines, transaction history, and token holdings.

  // Get wallet balances with USD conversion
  const address = 'rAddress...';
  const res = await fetch(`https://api.xrpl.to/v1/account/${address}/balances`);
  const balances = await res.json();

  balances.forEach(b => console.log(b.token, b.amount, '$' + b.usd_value));

  NFT Analytics

  Collection data, floor prices, trait rarity, sales history, and trader leaderboards.

  // Get NFT collection analytics
  const res = await fetch('https://api.xrpl.to/v1/nft/collections?sort=volume');
  const { collections } = await res.json();

  collections.forEach(c => {
    console.log(c.name, 'Floor:', c.floor_price, 'Vol:', c.total_volume);
  });

  WebSocket Streams

  Live price feeds, OHLC updates, orderbook changes, trade events, and ledger updates through a single connection.

  const ws = new WebSocket('wss://api.xrpl.to/ws');

  ws.onopen = () => {
    ws.send(JSON.stringify({
      method: 'subscribe',
      channels: ['tokens']
    }));
    console.log('Streaming live token updates...');
  };

  ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    console.log(data.name, '$' + data.price, data.change_24h + '%');
  };

  Risk Assessment

  Token review scores, scam detection, and creator activity tracking — data that doesn't exist in the raw ledger.

  // Check token risk score
  const res = await fetch('https://api.xrpl.to/v1/token/{id}/review');
  const review = await res.json();
  console.log('Risk score:', review.score, '/100');
  console.log('Flags:', review.flags);

  Endpoint Categories

  ┌───────────┬──────────────────────────────────────────────────┬──────────────────────────────┐
  │ Category  │                  What It Covers                  │      Example Endpoints       │
  ├───────────┼──────────────────────────────────────────────────┼──────────────────────────────┤
  │ Tokens    │ Prices, OHLC, holders, RSI, sparklines, trending │ /v1/tokens, /v1/token/{id}   │
  ├───────────┼──────────────────────────────────────────────────┼──────────────────────────────┤
  │ Trading   │ Orderbooks, AMM pools, quotes, trade history     │ /v1/orderbook, /v1/amm/pools │
  ├───────────┼──────────────────────────────────────────────────┼──────────────────────────────┤
  │ Accounts  │ Balances, trustlines, tx history, holdings       │ /v1/account/{addr}/balances  │
  ├───────────┼──────────────────────────────────────────────────┼──────────────────────────────┤
  │ NFTs      │ Collections, floors, traits, sales, rankings     │ /v1/nft/collections          │
  ├───────────┼──────────────────────────────────────────────────┼──────────────────────────────┤
  │ Risk      │ Scam detection, token scores, creator tracking   │ /v1/token/{id}/review        │
  ├───────────┼──────────────────────────────────────────────────┼──────────────────────────────┤
  │ WebSocket │ Live prices, trades, orderbook, ledger events    │ wss://api.xrpl.to/ws         │
  ├───────────┼──────────────────────────────────────────────────┼──────────────────────────────┤
  │ Utilities │ Health checks, faucet, embeddable charts         │ /v1/health                   │
  └───────────┴──────────────────────────────────────────────────┴──────────────────────────────┘

  Total: 232 endpoints

  Rate Limits & Pricing

  ┌──────────────┬───────┬───────────────┬──────────────┬────────┐
  │     Tier     │ Price │ Credits/Month │ Requests/Sec │ Tx/Sec │
  ├──────────────┼───────┼───────────────┼──────────────┼────────┤
  │ Anonymous    │ Free  │ Unlimited     │ 100          │ 1      │
  ├──────────────┼───────┼───────────────┼──────────────┼────────┤
  │ Free         │ $0    │ 1M            │ 10           │ 1      │
  ├──────────────┼───────┼───────────────┼──────────────┼────────┤
  │ Developer    │ $49   │ 10M           │ 50           │ 5      │
  ├──────────────┼───────┼───────────────┼──────────────┼────────┤
  │ Business     │ $499  │ 100M          │ 200          │ 50     │
  ├──────────────┼───────┼───────────────┼──────────────┼────────┤
  │ Professional │ $999  │ 200M          │ 500          │ 100    │
  └──────────────┴───────┴───────────────┴──────────────┴────────┘

  Anonymous access requires no API key and no sign-up. Just make requests.

  Authenticated tiers use an X-Api-Key header. Payments accepted in XRP or via Stripe.

  Why xrpl.to Instead of Raw Ledger APIs

  The XRP Ledger node API gives you raw ledger data — offers in drops, trustlines without context, transactions without enrichment.
  Building anything user-facing means writing your own indexer.

  xrpl.to handles all the indexing, aggregation, and calculation:

  - Token prices — calculated from live DEX activity, converted to USD
  - OHLC candles — aggregated from trade history at multiple timeframes
  - Volume and market cap — computed continuously across all trading pairs
  - Holder analytics — trustline crawling and distribution analysis
  - NFT floor prices — tracked across all marketplace activity
  - Risk scores — pattern detection and creator analysis
  - WebSocket streams — clean typed events instead of raw transaction metadata

  The platform has served over 61 million API requests across 162 countries.

  Why the XRP Ledger

  If you're evaluating which chain to build on:

  - Native DEX — order book built into the protocol, not a smart contract
  - Native AMM — protocol routes trades to whichever gives better price
  - No smart contract risk — trading happens at the ledger level
  - No MEV — consensus mechanism prevents transaction reordering
  - 3-5 second finality — deterministic, not probabilistic
  - Near-zero fees — fractions of a penny per transaction
  - 100% uptime — no outage since the ledger launched in 2012
  - Token issuance in one transaction — no contract deployment needed
  - Protocol-level NFTs — XLS-20 standard, no contract required

  Resources

  - API Documentation: https://xrpl.to/docs
  - Live DEX Data: https://xrpl.to
