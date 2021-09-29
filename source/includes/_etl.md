# ETLs

<aside class="notice">
The following is a table of all ETLs currently moving data from a source system into the Kasha data warehouse.
Some of these ETLs exist as PHP versions only, some as Python only, and some as both.
Where both versions of an ETL exist, the table indicates which one currently provides the data that reports and analyses are built upon.
Additionally, the table contains the names of the target columns in the data_warehouse that are being updated by the respective ETLs.
</aside>
<br>
<details>
<summary>List of all available ETLs, click to expand</summary>

| **source system for the ETL** | **PHP ETL available** | **Python ETL available**  | **reporting source**  | **SQL tables**
|-------------------------------|-----------------------|---------------------------|-----------------------|----------------|
| Woocommerce             | TRUE | TRUE | PHP    | <html><body><ul><li>PHP</li><ul><li>KASHA_DWH.woocommerce_orders</li><li>KASHA_DWH.woocommerce_order_line_items</li><li>KASHA_DWH.woocommerce_order_shipings</li><li>KASHA_DWH.woocommerce_order_refunds</li><li>KASHA_DWH.woocommerce_order_fees</li><li>KASHA_DWH.woocommerce_product_categories</li></ul><li>Python</li><ul><li>KASHA_DWH.python_woocommerce_orders</li><li>KASHA_DWH.python_woocommerce_order_meta_data</li><li>KASHA_DWH.python_woocommerce_order_line_items</li><li>KASHA_DWH.python_woocommerce_order_shiping_lines</li><li>KASHA_DWH.python_woocommerce_order_fee_lines</li><li>KASHA_DWH.python_woocommerce_product_categories</li></ul></ul></body></html> |
| Tradegecko              | TRUE | TRUE | Python | <html><body><ul><li>PHP</li><ul><li>KASHA_DWH.tradegecko_orders</li><li>KASHA_DWH.tradegecko_order_line_items</li></ul><li>Python</li><ul><li>KASHA_DWH.new_tradegecko_orders</li><li>KASHA_DWH.new_tradegecko_order_line_items</li></ul></ul></body></html> |
| ...                     |      |      |        | |
| ...                     |      |      |        | |
</details>

### Woocommerce orders
To call the woocommerce orders ETL for a specific date range, you first need to connect to the server via SSH

`ssh ubuntu@172.31.29.226 -i *your_ssh_key*.pem`
> Make sure to replace `*your_ssh_key*` with the filepath to your SSH key.
```shell
# For the PHP ETL navigate to the following directory
cd /home/ubuntu/data_warehouse/php/kasha-etl-rds

# in this directory, for a list of options, type
php artisan

# to call the Woocommerce orders ETL, call
php artisan etl:wc:orders <country> <start_date>
# where <country> and <start_date> are parameters to specify for which country (kenya or rwanda, mandatory argument),
# and for which start_date (optional argument) you want to call the ETL.
```

```shell
# For the Python ETL you first need to switch to user robot
sudo su robot

# and then navigate to the following directory
cd /home/robot/data_warehouse

# here you need to activate the virtual environment
. venv/bin/activate

# and then extend the python path
export PYTHONPATH="$PYTHONPATH:/home/robot/data_warehouse"

# here you can call the various python ETLs, with the example of the woocommerce orders etl being:
python ETL/woocommerce_etl_function.py <country> <start_date> <end_date>
# where <country> and <start_date> are parameters to specify for which country (kenya or rwanda, mandatory argument),
# and for which date interval (optional argument) you want to call the ETL. If the date interval is omitted, the default
# values of `today` and `14 days ago` will be used. 
```
