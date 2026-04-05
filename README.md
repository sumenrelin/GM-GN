# GM-GN
GM&amp;GN
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;
event ThankYouReceived(
    address indexed supporter,
    string message,
    uint256 amount,
    uint256 timestamp
);

constructor() {
    creator = msg.sender;   // デプロイした人が唯一の受益者です
}

// 投げ銭 + メッセージ（最低 0.0001 ETH）
function sendThankYou(string memory _message) external payable {
    require(msg.value >= 0.0001 ether, unicode"投げ銭金額は少なくとも 0.0001 ETH 必要です");
    require(bytes(_message).length > 0 && bytes(_message).length <= 280, 
            unicode"メッセージは1〜280文字以内で入力してください（日本語対応）");

    // 即時に100%を作成者（あなた）に転送
    (bool sent, ) = creator.call{value: msg.value}("");
    require(sent, unicode"送金に失敗しました");

    emit ThankYouReceived(msg.sender, _message, msg.value, block.timestamp);
}

// コントラクト情報を確認（任意）
function getInfo() external view returns (address creatorAddr, uint256 minAmount) {
    return (creator, 0.0001 ether);
}
    }
}
