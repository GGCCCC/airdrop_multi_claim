// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;

interface airdrop {
    function transfer(address recipient, uint256 amount) external;
    function balanceOf(address account) external view returns (uint256);
    function claim() external;
}
contract multiCall {
    address constant contra = address(0xbb2A2D70d6a4B80FA2C4d4Ca43a8525da430196c);
    function call(uint256 times) public {
        for(uint i=0;i<times;++i){
            new claimer(contra);
        }
    }
}
contract claimer{
    constructor(address contra){
        airdrop(contra).claim();
        uint256 balance = airdrop(contra).balanceOf(address(this));
        // require(balance>0,'Oh no');
        airdrop(contra).transfer(address(tx.origin), balance);
        selfdestruct(payable(address(msg.sender)));
    }
}