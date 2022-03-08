

https://github.com/ethereum/homebrew-ethereum/commits/master/solidity.rb

```
brew tap ethereum/ethereum
brew install solidity
brew unlink solidity
# Install 0.5.6 commit hash
brew install https://raw.githubusercontent.com/ethereum/homebrew-ethereum/1ecf6c60875740133ee51f6167aef9a4f05986e7/solidity.rb


brew extract --version='version_no' <package_name> <tap_name>
Example: brew extract --version='1.9' haproxy homebrew/core
```
잘 안됨