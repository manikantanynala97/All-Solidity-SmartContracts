// SPDX-License-Identifier: MIT

pragma solidity ^0.8.7;

contract Airline_Booking_ParentContract
{
   Airline_Booking_ChildContract public child;
   constructor()
   {
      //Create a new Child contract
      child = new Airline_Booking_ChildContract(msg.sender);
   }

}

contract Airline_Booking_ChildContract
{

    uint256 public constant FirstClassPrice = 0.01 ether;
    uint256 public constant BussinessClassPrice = 0.007 ether;
    uint256 public constant EconomyClassPrice = 0.005 ether;
    uint256 public TotalNoofBookedTickets ;

    address public owner ;

    struct Details
    {
        string Name;
        string Destination;
        uint256 PassportID;
        uint8 WhichClass;
    }

    enum Class
    {
        FirstClass,
        BussinessClass,
        EconomyClass
    }

    mapping(uint256 =>Details) People_Details;
    mapping(address => bool) AllowedAddress;

    modifier OnlyOwner()
    {
        require(msg.sender == owner , "Only owner is allowed to use this function");
        _;
    }

    constructor(address _owner )
    {
        owner = _owner; 
    }

   function BookTicket(string memory _Name , string memory _Destination , uint256 _PassportId, Class BookingClass) public payable returns(bool)
   {
      if(BookingClass == Class.FirstClass)
      {
          require(msg.value == FirstClassPrice , "Send the right amount to book your FirstClassTicket");
      }
      else if(BookingClass == Class.BussinessClass)
      {
          require(msg.value == BussinessClassPrice , "Send the right amount to book your BussinessClassTicket");

      }
      else if(BookingClass == Class.EconomyClass)
      {
          require(msg.value == EconomyClassPrice , "Send the right amount to  book your EconomyClassTicket");
      }

      People_Details[TotalNoofBookedTickets] = Details(
          _Name,
          _Destination,
          _PassportId,
          uint8(BookingClass)
      );

      TotalNoofBookedTickets+=1;

      return true;
   }

   function AddAddress(address _person) public OnlyOwner
   { 
       require(_person != address(0) , "Send a non zero address");
       require(AllowedAddress[_person] == false , "Already added to the allowed list of addresses");
       AllowedAddress[_person] = true;
   }

   function RemoveAddress(address _person) public OnlyOwner
   {
       require(_person != address(0) , "Send a non zero address");
       require(AllowedAddress[_person] == true , "The address is not in the allowed list of addresses");
       AllowedAddress[_person] = false;
   }

   function GetBalanceOfSmartContract() public view returns(uint256)
   {
       return address(this).balance;
   }

   function ReceiveEther() public OnlyOwner
   {
       address _owner = owner ;
       uint256 amount = address(this).balance;
       (bool sent,) = payable(_owner).call{value:amount}("");
       require(sent,"Ether not send succesfully");
   }

    receive() external payable {}

    fallback() external payable{}

}
