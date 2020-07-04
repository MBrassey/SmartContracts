# Solidity Smart Contracts

[Profile.sol](https://github.com/luc1dLife/SmartContracts/blob/master/Profile.sol) [waviii.io user profile storage contract]

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

<br />
[wavSwap.sol](https://github.com/luc1dLife/SmartContracts/blob/master/WavSwap.sol) [waviii.io wavSwap contract]

    pragma solidity ^0.5.0;

    import "./waviii.sol";

    contract EthSwap {
      string public name = "wavSwap";
      Token public token;
      uint public rate = 100;

      event TokensPurchased(
        address account,
        address token,
        uint amount,
        uint rate
      );

      event TokensSold(
        address account,
        address token,
        uint amount,
        uint rate
      );

      constructor(Token _token) public {
        token = _token;
      }

      function buyTokens() public payable {
        // Calculate the number of waviii tokens to buy
        uint tokenAmount = msg.value * rate;

        // Require that WavSwap has enough tokens
        require(token.balanceOf(address(this)) >= tokenAmount);

        // Transfer waviii tokens to the user
        token.transfer(msg.sender, tokenAmount);

        // Emit an event
        emit TokensPurchased(msg.sender, address(token), tokenAmount, rate);
      }

      function sellTokens(uint _amount) public {
        // User can't sell more tokens than they have
        require(token.balanceOf(msg.sender) >= _amount);

        // Calculate the amount of Ether to redeem
        uint etherAmount = _amount / rate;

        // Require that WavSwap has enough Ether
        require(address(this).balance >= etherAmount);

        // Perform swap
        token.transferFrom(msg.sender, address(this), _amount);
        msg.sender.transfer(etherAmount);

        // Emit an event
        emit TokensSold(msg.sender, address(token), _amount, rate);
      }

    }