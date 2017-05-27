---
layout: post
title: DRY bundler in capistrano
author: Richard Huang
description: There are a few posts told you how to integrate bundler into capistrano, but they are out of date now. After bundler 1.0 released, you can add only one line in capistrano to use bundler.
tags:
- deployment
- capistrano
- bundler
---
There are a few posts told you how to integrate Bundler into capistrano, the code snippet is as follows

    namespace :bundler do
      task :create_symlink, :roles => :app do
        shared_dir = File.join(shared_path, 'bundle')
        release_dir = File.join(current_release, '.bundle')
        run("mkdir -p #{shared_dir} && ln -s #{shared_dir} #{release_dir}")
      end

      task :bundle_new_release, :roles => :app do
        bundler.create_symlink
        run "cd #{release_path} && bundle install --without development test"
      end
    end

    after 'deploy:update_code', 'bundler:bundle_new_release'

But they are out of date after bundler 1.0 released. You don't need add such codes into your cap file. Now you can add only one line in capistrano to use the bundler

    require 'bundler/capistrano'

Bundler 1.0 extracts the capistrano tasks, so we don't need repeat it any more. Besides this, it uses --deployment option that avoids the permission issues on server.

Thanks Bundler makes gem dependencies much easier.
