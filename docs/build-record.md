# Build record

Policy name: BoundaryProofCeiling
Policy ARN: arn:aws:iam::111122223333:policy/BoundaryProofCeiling
Role name: BoundaryProofRole
Role ARN: arn:aws:iam::111122223333:role/BoundaryProofRole

Configuration evidence after boundary attachment:

{
    "Role": {
        "Path": "/",
        "RoleName": "BoundaryProofRole",
        "RoleId": "AROADBQP57FF2AEXAMPLE",
        "Arn": "arn:aws:iam::111122223333:role/BoundaryProofRole",
        "CreateDate": "2026-09-13T19:00:12+00:00",
        "AssumeRolePolicyDocument": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Sid": "TrustEc2WithoutInstanceProfile",
                    "Effect": "Allow",
                    "Principal": {
                        "Service": "ec2.amazonaws.com"
                    },
                    "Action": "sts:AssumeRole"
                }
            ]
        },
        "MaxSessionDuration": 3600,
        "PermissionsBoundary": {
            "PermissionsBoundaryType": "Policy",
            "PermissionsBoundaryArn": "arn:aws:iam::111122223333:policy/BoundaryProofCeiling"
        },
        "RoleLastUsed": {}
    }
}

The role has no attached managed permissions policy and no inline permissions policy. widened.json remains local simulator input only.