# Solidity Smart Contracts

[Profile.sol](https://github.com/luc1dLife/SmartContracts/blob/master/Profile_0.5.0.sol) waviii.io user profile storage contract

    pragma solidity ^0.5.0;

    contract Profile {

        struct User {
           string name;
           string subtitle;
           string sdescription;
           string ldescription;
           string weburl;
           string imghash;
        }

        mapping(address => User) public users; 

      function set(string memory _name, string memory _subtitle, string memory _sdescription,string memory _ldescription, string memory _weburl, string memory _imghash) public {
        users[msg.sender] = User(_name, _subtitle, _sdescription, _ldescription, _weburl, _imghash);
      }

      function get() public view returns (string memory, string memory, string memory, string memory, string memory, string memory) {
        return (
            users[msg.sender].name, 
            users[msg.sender].subtitle, 
            users[msg.sender].sdescription, 
            users[msg.sender].ldescription, 
            users[msg.sender].weburl, 
            users[msg.sender].imghash
        );
    }}