# Thuc's Notes website

![Deploy github page](https://github.com/ledongthuc/ledongthuc.github.io/workflows/Deploy%20github%20page/badge.svg)

## Notes

 - Source in `source` branch
 - Never merge `source` to `develop`

## Setup locally

1. Checkout source

```
git clone git@github.com:ledongthuc/ledongthuc.github.io.git;
cd ledongthuc.github.io.git;
git checkout source;
```

2. Run locally

```
hugo server;
curl http://localhost:1313/;
```

3. Deploy to https://thuc.space

Push your changes to `source` branch. It will automatically to build and deploy `master` branch and https://thuc.space
