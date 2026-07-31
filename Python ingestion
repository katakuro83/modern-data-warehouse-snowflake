"""
load_raw_to_snowflake.py

Loads local CSV files into the RAW schema in Snowflake via an internal
stage + COPY INTO. This mirrors how a lightweight ingestion job would
work; in production this step is typically replaced by Snowpipe,
Fivetran, Airbyte, or a CDC tool, but the RAW-schema contract stays
the same either way.

Usage:
    python load_raw_to_snowflake.py

Requires environment variables (or a .env file loaded beforehand):
    SNOWFLAKE_ACCOUNT
    SNOWFLAKE_USER
    SNOWFLAKE_PASSWORD
    SNOWFLAKE_ROLE       (default: DWH_LOADER)
    SNOWFLAKE_WAREHOUSE  (default: DWH_WH)
    SNOWFLAKE_DATABASE   (default: MODERN_DWH)
    SNOWFLAKE_SCHEMA     (default: RAW)
"""

import os
import sys
import logging
from pathlib import Path

import snowflake.connector

logging.basicConfig(level=logging.INFO, format="%(asctime)s | %(levelname)s | %(message)s")
logger = logging.getLogger(__name__)

RAW_DATA_DIR = Path(__file__).parent.parent / "data" / "raw"

# Map local file -> target raw table
FILE_TABLE_MAP = {
    "customers.csv": "CUSTOMERS",
    "products.csv": "PRODUCTS",
    "orders.csv": "ORDERS",
}


def get_connection():
    required = ["SNOWFLAKE_ACCOUNT", "SNOWFLAKE_USER", "SNOWFLAKE_PASSWORD"]
    missing = [v for v in required if not os.environ.get(v)]
    if missing:
        logger.error("Missing required environment variables: %s", ", ".join(missing))
        sys.exit(1)

    return snowflake.connector.connect(
        account=os.environ["SNOWFLAKE_ACCOUNT"],
        user=os.environ["SNOWFLAKE_USER"],
        password=os.environ["SNOWFLAKE_PASSWORD"],
        role=os.environ.get("SNOWFLAKE_ROLE", "DWH_LOADER"),
        warehouse=os.environ.get("SNOWFLAKE_WAREHOUSE", "DWH_WH"),
        database=os.environ.get("SNOWFLAKE_DATABASE", "MODERN_DWH"),
        schema=os.environ.get("SNOWFLAKE_SCHEMA", "RAW"),
    )


def load_file(cursor, filename: str, table: str) -> None:
    local_path = RAW_DATA_DIR / filename
    if not local_path.exists():
        raise FileNotFoundError(f"Expected raw file not found: {local_path}")

    logger.info("Staging %s -> @RAW_STAGE", filename)
    # PUT uploads the local file to the internal stage. overwrite=true keeps
    # re-runs idempotent for this demo; in production you'd use unique
    # filenames + a load history table instead of overwriting.
    cursor.execute(
        f"PUT file://{local_path} @RAW_STAGE OVERWRITE = TRUE AUTO_COMPRESS = TRUE"
    )

    logger.info("Loading %s into RAW.%s", filename, table)
    cursor.execute(
        f"""
        COPY INTO RAW.{table}
        FROM @RAW_STAGE/{filename}.gz
        FILE_FORMAT = (FORMAT_NAME = 'CSV_STANDARD')
        ON_ERROR = 'ABORT_STATEMENT'
        """
    )
    result = cursor.fetchall()
    logger.info("Load result for %s: %s", table, result)


def main():
    conn = get_connection()
    try:
        cursor = conn.cursor()
        for filename, table in FILE_TABLE_MAP.items():
            load_file(cursor, filename, table)
        logger.info("All raw files loaded successfully.")
    finally:
        conn.close()


if __name__ == "__main__":
    main()
