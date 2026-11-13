# Data Dictionary — All Columns, All Tables

All timestamps IST, all money in ₹, blank = not applicable/not populated.
Join key across all files: `third_party_id`.

---

## users.csv (one row per user)

| Column | Meaning |
|---|---|
| `third_party_id` | User identifier (UUID); join key to all order tables |
| `signup_ts` | When the user created their Groww account |
| `signup_platform` | Surface used at signup: android / ios / web / msite |
| `kyc_at_signup` | User's KYC status at the moment of signup (point-in-time) |
| `kyc_status` | Current KYC verification state |
| `kra_flag` | Outcome of the first KRA check: kra_verified / kra_not_verified / kra_not_checked (KYC pre-existing with another broker) |
| `gender` | Self-declared gender; blank for many users |
| `age` | Age in years at signup, derived from date of birth |
| `income` | Self-declared annual income bracket (a. < 1 Lakh … h. > 5 Crore) |
| `occupation` | Self-declared occupation (STUDENT, PRIVATE_SECTOR_SERVICE, …) |
| `kyc_city` | City from the user's KYC address |
| `kyc_state` | State from the user's KYC address |
| `kyc_tier` | City tier of the KYC city: Metro + 2 / Tier - 2,3 / Tier - 4 / '-' |
| `utm_source` | Raw UTM attribution: source of the signup visit |
| `utm_medium` | Raw UTM attribution: medium of the signup visit |
| `utm_campaign` | Raw UTM attribution: campaign of the signup visit |
| `acquisition_channel` | Mapped acquisition channel (level 1: google adwords, organic, ad partner, referral, …) |
| `acquisition_subchannel` | Mapped acquisition sub-channel (level 2: campaign-level string) |
| `first_esign_ts` | When the user's first e-sign (demat opening step) was submitted |
| `aof_ts` | When the Account Opening Form (PoA) was completed |
| `stock_onboarding_ts` | When stocks (broking) onboarding completed (exchange registration done) |
| `first_bank_added_ts` | When the user's first bank account was verified |
| `selfie_verified_ts` | When the selfie (IPV) verification was approved |
| `digilocker_status` | DigiLocker document-sourcing step: ACCEPTED / NOT_SUBMITTED / PARTIAL / SKIPPED / … |
| `fid_ts` | First Investment Date — first successful investment (excludes gold/GB and IPO applications); blank if never invested |
| `fid_product` | Product of that first investment: cnc/mis/mtf (stocks), sip/lumpsum (MF), fno, commodity, … |
| `bank_name` | User's primary bank |

---

## stocks_orders.csv (one row per equity/ETF order)

| Column | Meaning |
|---|---|
| `third_party_id` | User identifier |
| `groww_order_id` | Unique Groww order id (primary key) |
| `created_at` | When the order was placed on Groww |
| `updated_at` | Last update to the order record |
| `exchange_creation_time` | When the exchange accepted the order |
| `exchange_update_time` | Last update reported by the exchange (typically execution) |
| `auto_square_off` | 1 if an intraday auto-square-off (forced end-of-day exit) |
| `buy_sell` | Direction: B = buy, S = sell |
| `is_amo` | 1 if an after-market order (placed outside market hours) |
| `order_type` | MARKET / LIMIT / SL (stop-loss) / SL-M |
| `product` | CNC (delivery) / MIS (intraday) / MTF (margin trading) |
| `exchange` | NSE / BSE |
| `symbol_isin` | ISIN of the security (INF… = ETF) |
| `symbol_name` | Trading symbol (RELIANCE, TCS, …) |
| `qty` | Quantity ordered (shares) |
| `filled_qty` | Quantity actually executed |
| `limit_price` | Limit price set by the user (₹) |
| `trigger_price` | Trigger price set by the user (₹, stop-loss orders) |
| `avg_fill_price` | Average execution price (₹) |
| `turnover_inr` | Executed value in ₹ (avg_fill_price × filled_qty); populated only for successful orders, else 0 |
| `order_platform` | Platform the order came from: android / ios / web / api |
| `order_status` | SUCCESS / CANCELLED / REJECTED / FAILED / OTHERS |
| `is_sip_order` | 1 if placed by a Stock SIP (systematic periodic buy) |
| `is_etf_order` | 1 if the security is an ETF |
| `is_target_order` | 1 if placed via the target-order flow (price-alert triggered) |
| `order_source` | Internal classification of the surface/flow that produced the order |
| `smart_order_id` | Smart-order (GTT) reference id, if the order was trigger-routed |
| `is_gtt` | 1 if a GTT (Good Till Triggered) order |
| `tr_name` | Trading/routing engine version that processed the order (v1, v2, …) |
| `charges` | Total statutory + transaction charges on the order (₹): STT, GST, exchange, SEBI, stamp duty |
| `original_brokerage` | Brokerage actually charged on the order (₹) |

---

## fno_orders.csv (one row per F&O order)

| Column | Meaning |
|---|---|
| `third_party_id` | User identifier |
| `pt` | Trade date of the order in the source system (date string) |
| `order_created_at` | When the order was placed on Groww |
| `order_updated_at` | Last update to the order record |
| `exchange_creation_time` | When the exchange accepted the order |
| `exchange_update_time` | Last update reported by the exchange (typically the execution time) |
| `groww_order_id` | Unique Groww order id |
| `contract_id` | Id of the derivative contract (instrument) traded |
| `order_status` | EXECUTED / CANCELLED / REJECTED / FAILED |
| `order_type` | MARKET / LIMIT / SL / SL-M |
| `product` | NRML (carry-forward) / MIS (intraday) |
| `amo_status` | After-market-order status, if placed outside market hours |
| `buy_sell` | Direction: B = buy, S = sell |
| `qty` | Quantity ordered (lots) |
| `filled_qty` | Quantity actually executed |
| `remaining_qty` | Quantity unfilled at last update |
| `avg_fill_price` | Average execution price (₹) |
| `platform` | Platform the order came from |
| `symbol_isin` | ISIN of the derivative contract |
| `contract_type` | FUT (futures) / OPT (options) |
| `call_put` | Option type: CE (call) / PE (put); blank for futures |
| `symbol_name` | Trading symbol of the contract |
| `order_source` | Internal classification of the surface/flow that produced the order |
| `is_basket_order` | 1 if placed as part of a basket |
| `is_fast_exit` | 1 if a quick position-close (fast exit) order |
| `is_exitall` | 1 if an "exit all positions" action |
| `contracts_underlying` | Underlying security/index of the contract |
| `contracts_expiry_date` | Expiry date of the contract |
| `contracts_strike_price` | Strike price (options) |
| `strike_interval` | Strike interval of the option chain |
| `underlying_price` | Price of the underlying when the order was placed |
| `contracts_min_lot_qty` | Minimum lot size of the contract |
| `contracts_freeze_qty` | Exchange freeze-quantity limit for the contract |
| `contracts_tick_size` | Minimum price movement (tick) for the contract |
| `contracts_exchange` | Exchange of the contract (NSE / BSE) |
| `contracts_instrument_type` | Exchange instrument code (FUTIDX, OPTSTK, …) |
| `api_flag` | 1 if the order came via API (algo/programmatic) |
| `is_tradingday` | 1 if the order date was a trading day |
| `smart_order_id` | Smart-order reference id, if trigger-routed |
| `gtt_order_id` | GTT trigger reference id, if trigger-based |
| `event_order_source` | Event/trigger that generated the order, if applicable |
| `strike_dist_from_ul` | Distance of the strike from the underlying price |
| `itm_atm_otm` | Option moneyness: ITM / ATM / OTM |
| `is_entry` | 1 if this order opened a position (vs closing one) |
| `pnl` | Realized profit/loss (₹, signed) attributed to this order's position |
| `position_exit_ts` | When the position opened by this order was exited |
| `position_exit_ts_last` | Most recent exit timestamp for the position (partial exits) |
| `exit_count` | Number of exits taken to close the position |
| `error_reasons_cleaned` | Cleaned rejection/failure reason, if the order failed |
| `brokerage_inr` | Brokerage earned (assumed flat): ₹20 if EXECUTED, else 0 |

---

## mf_orders.csv (one row per mutual fund order)

| Column | Meaning |
|---|---|
| `third_party_id` | User identifier |
| `created_at` | When the order was placed on Groww |
| `updated_at` | Last update to the order record |
| `groww_order_id` | Unique Groww order id |
| `groww_order_prefix` | Prefix component of the order id (internal format) |
| `exchange_order_id` | Order id on the exchange side (BSE) |
| `com_order_id` | Common order id linking related order rows (internal) |
| `cart_id` | Cart id, if placed as part of a multi-scheme cart |
| `order_status` | COMPLETED / PAYMENT_FAILED / CANCELLED / REJECTED / APPROVED / PAUSED |
| `order_approved_at` | When the order was approved for processing |
| `order_completed_at` | When the order completed (units allotted) |
| `order_rejected_at` | When the order was rejected, if rejected |
| `order_closed_at` | When the order was closed, if closed |
| `bse_order_punch_date` | Date the order was punched to BSE (MF orders route via BSE/RTA) |
| `actual_nav_date` | NAV date applied to the order |
| `settlement_date` | Settlement date of the transaction |
| `expected_completion_date` | Expected completion date at placement |
| `order_type` | Order type classification (e.g. NORMAL) |
| `order_kind` | Kind of order: SIP / LUMPSUM |
| `order_subkind` | Finer kind (e.g. SIP first installment vs normal) |
| `buy_sell` | P = purchase, R = redeem |
| `buy_sell_type` | Additional direction classification |
| `settlement_type` | Settlement mode of the transaction |
| `sip_flag` | 1 if a SIP order |
| `is_liquid` | 1 if a liquid/short-duration fund order |
| `is_nfo` | 1 if a new fund offering (NFO) order |
| `is_first_order` | 1 if the first order of its kind for the user |
| `all_redeem` | 1 if a full (all-units) redemption |
| `groww_sip_id` | Id of the SIP this order belongs to, if a SIP |
| `order_val` | Order value (₹) |
| `placed_amount` | Amount actually placed/planned for the order (₹) |
| `qty` | Units transacted, where applicable |
| `payment_source` | Payment source (Groww Balance, net banking, UPI, …) |
| `scheme_name` | Name of the mutual fund scheme |
| `scheme_code` | Groww scheme code |
| `isin` | ISIN of the scheme |
| `scheme_benchmark_index` | Benchmark index of the scheme |
| `bse_scheme_code` | BSE scheme code |
| `refund_mode` | Refund mode, for failed/redemption flows |
| `payment_mode_type` | Payment mode classification |
| `payment_initiated_at` | When payment was initiated |
| `payment_ref_id` | Payment reference id |
| `payment_type` | Payment type classification |
| `checkout_type` | Checkout flow used for payment |
| `payment_status` | Final payment status |
| `payment_completed_at` | When payment completed, if completed |
| `payment_failed_at` | When payment failed, if failed |
| `total_payment_attempts` | Total payment attempts for the order |
| `total_payment_attempts_via_checkout` | Payment attempts via the checkout flow |
| `total_comp_payment_via_checkout` | Completed payments via checkout |
| `total_fail_payment_via_checkout` | Failed payments via checkout |
| `total_payment_attempts_via_presentation` | Payment attempts via the presentation flow |
| `total_comp_payment_via_presentation` | Completed payments via presentation |
| `total_fail_payment_via_presentation` | Failed payments via presentation |
| `overall_fid_flag` | Internal flag: this order is the user's overall first investment |
| `mf_fid_flag` | Internal flag: this order is the user's first MF investment |
| `order_kind_fid_flag` | Internal flag: first investment of this order kind |
| `rejection_remark_type` | Classification of the rejection reason, if rejected |
| `exchange` | Routing exchange (BSE / RTA) |
| `brokerage_inr` | Fee earned (assumed flat): ₹2 if COMPLETED, else 0 |

---

## ipo_orders.csv (one row per IPO application)

| Column | Meaning |
|---|---|
| `third_party_id` | User identifier |
| `groww_order_id` | Unique Groww application id |
| `exchange_order_id` | Application id on the exchange side |
| `created_at` | When the application was placed |
| `updated_at` | Last update to the application record |
| `isin` | ISIN of the IPO issue |
| `symbol` | Symbol of the IPO issue |
| `category` | Application category: IND (retail incl. SHNI) / SHA (shareholder) / EMP (employee) |
| `new_payment_success_flag` | 1 if the payment (UPI mandate) succeeded |
| `allotment_doneflag` | 1 if shares were allotted |
| `upi_amount_blocked` | Amount blocked via the UPI mandate (₹) |
| `shares_allocated` | Number of shares allocated (0 if not allotted) |
| `name` | Name of the IPO company |
| `lot_size` | Shares per lot of the issue |
| `max_bid_value` | Maximum bid value of the application (₹) |
| `start_date` | Issue subscription open date |
| `end_date` | Issue subscription close date |
| `max_price` | Upper price band of the issue (₹) |
| `min_price` | Lower price band of the issue (₹) |
| `is_sme` | 1 if an SME-platform IPO |
| `min_bid_quantity` | Minimum bid quantity (lots) |
| `face_value` | Face value per share (₹) |
| `os_name` | Operating system of the device used (Android / iOS) |
| `order_status` | Final status: ALLOTMENT_DONE / NOT_ALLOTED / CANCELLED / REJECTED / APPROVED / NEW / … |
| `payment_status` | Payment status: PENDING / SUCCESS / BLOCKED_RELEASED / FAILED |
| `new_order_time` | When the application moved to NEW status |
| `approved_time` | When the application was approved for the exchange |
| `cancellation_requested_time` | When cancellation was requested, if cancelled |
| `cancelled_time` | When cancellation completed, if cancelled |
| `not_alloted_time` | When non-allotment was recorded |
| `failed_time` | When the application failed, if failed |
| `allotment_done_time` | When allotment completed |
| `rejected_time` | When the application was rejected, if rejected |
| `payment_pending_time` | When payment went to pending, if applicable |
| `payment_success_time` | When payment succeeded |
| `payment_failed_time` | When payment failed, if failed |
| `mandate_status` | UPI mandate status |
| `mandate_created_at` | When the UPI mandate was created |
| `mandate_received_at` | When the mandate was received by the exchange |
| `mandate_amount` | Mandate amount (₹) |
| `mandate_error_reason` | Mandate error reason, if errored |
| `bank_name` | Bank used for the mandate |
| `bank_id` | Bank identifier |
| `mandate_action_taken` | Mandate action taken (approve / decline) |
| `mandate_approved_at` | When the mandate was approved |
| `mandate_revoked_at` | When the mandate was revoked |
| `discovery_source` | Where in the app the IPO was discovered from |
| `mandate_receive_absolute_tat` | Absolute turnaround time for mandate receipt |
| `mandate_receive_modified_tat` | Business-hours-adjusted turnaround time for mandate receipt |
| `mandate_set` | 1 if a mandate was set up |
| `approve_click` | 1 if the user clicked approve on the mandate |
| `decline_click` | 1 if the user clicked decline on the mandate |
| `mandate_discovered` | 1 if an existing mandate was discovered/reused |
| `gupi_alloted_at` | Allotment timestamp from the GUP (Groww UPI) flow |
| `brokerage_inr` | Fee earned (assumed flat): ₹1 if successfully applied (alloted or not alloted), else 0 |

---

## commodity_orders.csv (one row per commodity derivative order)

| Column | Meaning |
|---|---|
| `third_party_id` | User identifier |
| `created_at` | When the order was placed on Groww |
| `updated_at` | Last update to the order record |
| `groww_order_id` | Unique Groww order id |
| `contract_id` | Id of the derivative contract traded |
| `gui_order_id` | Internal order-system id |
| `exchange_order_id` | Order id on the exchange (MCX) |
| `trigger_price` | Trigger price set by the user (₹, stop-loss orders) |
| `trading_symbol` | Trading symbol of the contract |
| `symbol_isin` | ISIN of the contract |
| `qty` | Quantity ordered (lots) |
| `filled_qty` | Quantity actually executed |
| `avg_fill_price` | Average execution price (₹) |
| `order_type` | MARKET / LIMIT / SL / SL-M |
| `buy_sell` | Direction: B = buy, S = sell |
| `order_status` | EXECUTED / CANCELLED / REJECTED / FAILED |
| `underlying` | Underlying commodity (GOLD, CRUDEOIL, …) |
| `future_option` | FUT (futures) / OPT (options) |
| `exchange_creation_time` | When the exchange accepted the order |
| `platform` | Platform the order came from |
| `call_put` | Option type: CE (call) / PE (put) |
| `entry_exit_flag` | Whether the order opened or closed a position |
| `remark` | Free-text remark on the order |
| `exchange` | Exchange (MCX) |
| `error_reasons` | Rejection/failure reason, if the order failed |
| `contract_expiry_date` | Expiry date of the contract |
| `brokerage_inr` | Brokerage earned (assumed flat): ₹20 if EXECUTED, else 0 |
