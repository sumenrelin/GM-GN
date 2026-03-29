# GM-GN
GM&amp;GN
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract BaseThankYouJar {
    address public immutable creator;

    event ThankYouReceived(
        address indexed supporter,
        string message,
        uint256 amount,
        uint256 timestamp
    );

    constructor() {
        creator = msg.sender;   // 部署者就是唯一受益人
    }

    // 打赏 + 留言（最低 0.0001 ETH）
    function sendThankYou(string memory _message) external payable {
        require(msg.value >= 0.0001 ether, unicode"打赏金额必须至少 0.0001 ETH");
        require(bytes(_message).length > 0 && bytes(_message).length <= 280, 
                unicode"留言必须在 1-280 个字符之间（支持中文）");

        // 立即 100% 转给创建者（你）
        (bool sent, ) = creator.call{value: msg.value}("");
        require(sent, unicode"转账失败");

        emit ThankYouReceived(msg.sender, _message, msg.value, block.timestamp);
    }

    // 查看合约信息（可选）
    function getInfo() external view returns (address creatorAddr, uint256 minAmount) {
        return (creator, 0.0001 ether);
    }
}
