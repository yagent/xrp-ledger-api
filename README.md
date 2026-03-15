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

  Beginner
  - Token price tracker using /v1/tokens and /v1/token/{id}
  - Portfolio dashboard using /v1/account/{address}/balances

  Intermediate
  - DEX trading interface using /v1/orderbook and /v1/amm/pools
  - Price alert bot using WebSocket tokens channel
  - NFT analytics tool using /v1/nft/collections and /v1/nft/floors

  Advanced
  - Arbitrage scanner using /v1/orderbook, /v1/amm/pools, and WebSocket
  - Trading bot using /v1/orderbook, /v1/submit, and WebSocket

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

  232 Endpoints Across 7 Categories

  Tokens — Prices, OHLC, holders, RSI, sparklines, trending, new listings

  Trading — Orderbooks, AMM pools, swap quotes, trade history, volume analytics

  Accounts — Balances, trustlines, transaction history, token holdings

  NFTs — Collections, floor prices, traits, sales history, trader rankings

  Risk — Scam detection, token review scores, creator activity tracking

  WebSocket — Live prices, trades, orderbook depth, ledger events, news feeds

  Utilities — Health checks, faucet access, embeddable charts

  Rate Limits & Pricing

  Anonymous — Free, no sign-up, no API key, 100 req/sec, unlimited credits

  Free — $0/month, 1M credits, 10 req/sec

  Developer — $49/month, 10M credits, 50 req/sec

  Business — $499/month, 100M credits, 200 req/sec

  Professional — $999/month, 200M credits, 500 req/sec

  Payments accepted in XRP or via Stripe. Each endpoint costs 1-20 credits per call.

  Why xrpl.to Instead of Raw Ledger APIs

  The XRP Ledger node API gives you raw ledger data — offers in drops, trustlines without context, transactions without enrichment.
  Building anything user-facing means writing your own indexer.

  xrpl.to handles all the indexing, aggregation, and calculation:

  - Token prices calculated from live DEX activity, converted to USD
  - OHLC candles aggregated from trade history at multiple timeframes
  - Volume and market cap computed continuously across all trading pairs
  - Holder analytics from trustline crawling and distribution analysis
  - NFT floor prices tracked across all marketplace activity
  - Risk scores from pattern detection and creator analysis
  - WebSocket streams as clean typed events instead of raw transaction metadata

  The platform has served over 61 million API requests across 162 countries.

  Why the XRP Ledger

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

  - https://xrpl.to/docs
  - https://xrpl.to
