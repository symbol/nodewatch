# nodewatch

Monitors network version upgrades.

For pull to work, certificates must be placed in puller/certs/
To produce certificates follow the instructions for generating keys and certs in the [symbol node guide](https://docs.symbolplatform.com/guides/network/running-a-symbol-node-manually.html).

```sh
# install miscellaneous dependencies
git submodule update --init
cd ./miscellaneous && python -m pip install -r requirements.txt && cd ..

# generate certificates and run puller
cd ./puller \
    && mkdir -p certs \
    && cd certs \
    && curl -O https://docs.symbolplatform.com/_static/bash/cert-generate.sh && chmod +x cert-generate.sh 

## save an arbitrary mainnet privatekey and generate certificates
echo 393C4BB001D6236FDB41F2D2E5DC3CEF5B939A83730F0AB7127935F2DDA24704 >> private.main.txt \
    && ./cert-generate.sh && cd ..

mkdir -p results && PYTHONPATH=../miscellaneous && ./pull.sh ./results 10 && cd ..

# install nodewatch dependencies & run the nodewatch app
python -m pip install -r requirements.txt

echo RESOURCES_PATH=\'./puller/results/\' > ./nodewatch/config.py
NODEWATCH_SETTINGS=config.py && FLASK_APP=nodewatch && flask run
```
